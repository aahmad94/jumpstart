---
layout: post
title: ruby strings exercise 
date: 2017-04-23 18:30:00
---

<h3>I finished the strings exercise from App Academy's JumpStart preparation curriculum.</h3>

<p>I want to focus on the "hard" set of questions. The solution was pretty straight forward for each method we had to define but on the last two prompts I got hung up on a few simple but crucial points.</p>

{% highlight ruby %}
# Write a method that returns an array of the digits of a non-negative integer in descending order and as strings, e.g., descending_digits(4291) #=> ["9", "4", "2", "1"]
def descending_digits(int)
  int.to_s.split("").sort.reverse
  # your code goes here
end
{% endhighlight %}

<p>The prompt here tells us that we're given an integer and that we want to (1) convert the integer into a string, (2) <i>split</i> the string into an array, (3) <i>sort</i> it, and (4) <i>reverse</i> the order so it's values are listed from low to high.</p>

{% highlight ruby %}
# Write a method that converts an array of ten integers into a phone number in the format "(123) 456-7890".
def to_phone_number(arr)
  num_str = arr.sort.join("")
  phone_number = "(" + num_str[1..3] + ") " + num_str[4..6] + "-" + num_str[7..9] + num_str[0]
  # your code goes here
end
{% endhighlight %}

<p>This one was a little more tough since I didn't account for a few simple things that were happening. We're asked to produce a phone number in the format <i>(123) 456-7890</i> give an array that has those numbers. So I started off thinking we can take the array and <i>sort</i> it and with that we can <i>join</i> together it's values to get a <strong>string</strong>. This was actually the right though process but here's how I defined my phone_number variable initially:</p>

<p><i>phone_number =  "(" + num_str[0..2] + ") " + num_str[3..5] + "-" + num_str[6..9]</i></p>

<p>This, however, would give me the output:</p>

<p><strong>(012) 345-6789</strong></p>

<p>The reason being is that our <strong>string</strong>, after being <i>sorted</i>, it is actually <i>"012346789"</i>; I hadn't accounted for the <i>0</i> at the beginning of the string.</p>


{% highlight ruby %}
# Write a method that returns the range of a string of comma-separated integers, e.g., str_range("4,1,8") #=> 7
def str_range(str)
  array_ordered = str.split(",").sort
  range = array_ordered[-1].to_i - array_ordered[0].to_i
  # your code goes here
end
{% endhighlight %}

<p>My mistake here was also a simple one; I forgot that when we <i>split</i> a string into an array, the elements in the array are all in there as strings. Since all the string values in the array are <i>numerical</i> (as opposed to numerical and alphabetical), we can <i>sort</i> the array without any issues.</p>

<p>My problem, however, came when we had to access elements from the array and use them in a mathematical operation to obtain the range. I tried to to set the range equal to <i>array_ordered[-1] - array_ordered[0]</i>. Unfortunately, we can't do that since the element being accessed is a string, we have to convert it into an integer, which I then did, with the <i>to_i</i> method.</p>

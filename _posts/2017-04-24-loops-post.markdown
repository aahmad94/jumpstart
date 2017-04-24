---
layout: post
title: ruby loops exercise 
date: 2017-04-24 18:00:00
---

<h3>Loops in loops!</h3>

<p>It can be hard to follow the logic when using outer and inner loops, so I tried to make my variables as descriptive as possible in my script. The prompt here asked to make a method that allows us to determine if any two elements within an array sum to zero.</p>

<h4>An outer loop will be used to pull each element in an array and test it against conditions given by an inner loop, which, in this case, will pull values for every <i>other</i> element in the array so we can check if any combination of elements from the array gives us the sum of zero.</h4>

{% highlight ruby %}
# Define a method that returns a boolean indicating whether any two elements in the argument (an array) sum to 0.
def test_if_sum_0(arr)
  
  test_element_count = 0
  test_element = arr[test_element_count]
  while test_element_count < arr.length()
   
    other_element_count = test_element_count + 1
    other_element = arr[other_element_count]
    while other_element_count < arr.length
      
      test = test_element + other_element == 0
      
      puts test if test == true
      
      other_element_count += 1
      
    end
    
    test_element_count += 1
    
  end
  
end
{% endhighlight %}

<p>My solution is above and doesn't <i>exactly</i> answer the prompt. My setup is analogous to that in the solution below but after I tested to see whether the sum of two elements is zero, I made it so the script would print "true" for each instance; below, they stopped the inner loop upon the first zero instance and returned a true value. I prefer to know how many true instances there are and as such I didn't use the <i>return</i> which stops the loop. Even though my setup works, I would answer the prompt exactly as it dictates if this were an assesment. The outer/inner loop scheme is pretty common and worth memorizing.</p>

{% highlight ruby %}
def two_sum_to_zero?(arr)
  i = 0
  while i < arr.length # outer loop
    num_one = arr[i]
    j = i + 1 # offset the inner counter by one so we don't count an element at the same position twice
    while j < arr.length # inner loop
      num_two = arr[j]
      if num_one + num_two == 0
        return true # our work here is done
      end
      j = j + 1
    end
    i = i + 1
  end
  # by now, we've checked every combination of elements
  # if we haven't returned true yet, then no two elements sum to 0
  false
end
{% endhighlight %}
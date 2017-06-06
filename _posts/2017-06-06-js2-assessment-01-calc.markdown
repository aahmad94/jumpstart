---
layout: post
title: jumpstart 2 assessment 01: calculator
date: 2017-06-04 11:08:00
---

<h4>Calculater<br>

You are given a hash with letters as keys and mathematical operators as values,
an input array of numbers and letters, and a starting value<br>

Write a method that goes takes the start value then goes through the input array
performing the operation on the accumulated output and up until that point
and the next number in the array. If a letter in the input does not appear in
the hash, skip it and the number after it<br>

Example:<br>
hash = {"a" => "+", "z" => "*", "t" => "/"}<br>
input = ["z", 5, "t", 8]<br>
start = 9<br>

calculater(hash, input, start) = 5 (remember integer division!)</h4>

<p>Hard coded solution:</p>
{% highlight ruby %}
def calculater(hash, input, start)
  count = 0
  while count < input.length
    if hash[input[count]] == "+"
      start = start + input[count+1]
    elsif hash[input[count]] == "*"
      start = start * input[count+1]
    elsif hash[input[count]] == "/"
      start = start / input[count+1]
    elsif hash[input[count]] == "-"
      start = start - input[count+1]
    end
    count += 1
  end
  start
end
{% endhighlight %}

<p>Using a newly learned method, <i>send</i>:</p>
{% highlight ruby %}
def calculater(hash, input, start)
  (1..input.length).step(2) do |index|
    if hash.key?(input[index - 1])
      start = start.send(hash[input[index - 1]], input[index])
    end
  end
  start
end
{% endhighlight %}

<p>These are the corresponding test cases that must pass as true:</p>
{% endhighlight %}
puts "---------Calculater---------"

hash = {"a" => "+", "z" => "*", "t" => "/"}
hash2 = { "y" => "*", "r" => "/", "u" => "-"}
puts calculater(hash, ["z", 5, "t", 8], 9) == 5
puts calculater(hash, ["z", 5, "z", 3], 3) == 45
puts calculater(hash2, ["a", 5, "y", 7, "r", 9, "u", 4], 8) == 2
puts calculater(hash2, ["y", 5, "u", 20, "r", 9, "y", 4], 0) == -12
{% highlight ruby %}

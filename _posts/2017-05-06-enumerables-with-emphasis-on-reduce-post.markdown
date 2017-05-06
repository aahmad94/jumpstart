---
layout: post
title: ruby enumerables with an emphasis on reduce
date: 2017-05-06 01:00:00
---

<h4>Enumerables primarily traverse or sort collections (ranges, arrays, and hashes) via iteration of a block of code; there's a few big ones: <i>each or each.with_index(), map or collect, all?, none?, any?, count, sort_by, select, and reject</i></h4>

<p>The <i>map</i> or <i>collect</i>, <i>select</i>, and <i>reject</i> methods are enumerables that return a new collection (unlike <i>each</i> or <i>each.with_index()</i>); you don't need to shovel the elements in a result array because these enumerables return a new array that's the result of executing its given block once for each element in its receiver.</p>

{% highlight ruby %}
# Example 1
simpleton = [1, 2, 3]
simpleton.map {|int| int*2} #=> [2,4,6]
simpleton #=> [1, 2, 3]

# Example 2
about_to_be_slightly_less_simpleton = [1, 2, 3]
about_to_be_slightly_less_simpleton.map! {|int| int**2 #=> [1, 4, 9]
about_to_be_slightly_less_simpleton #=> [1, 4, 9]

# Example 3 (select and reject)
array_of_terms = ["The blab of the pave", "tires of carts",
      "sluff of boot-soles", "talk of the promenaders",
      "The heavy omnibus", 'the driver with his interrogating thumb']

  array_of_terms.select {|t| t.length > 20} #=> ["talk of the promenaders", "the driver with his interrogating thumb"]
  array_of_terms.reject {|t| t.length > 20} #=> ["The blab of the pave", "tires of carts", "sluff of boot-soles",
                                            #    "The heavy omnibus"]

  # WELCOME TO THE DANGER ZONE
  array_of_terms.select! {|t| t.length > 20} #=> ["talk of the promenaders", "the driver with his interrogating thumb"]
  array_of_terms #=> ["talk of the promenaders", "the driver with his interrogating thumb"]
{% endhighlight %}

<p>This is in contrast to the <i>each</i> method:</p>

{% highlight ruby %}
katz_deli = ["rick", "morty"]

def line(katz_deli)
  
  line_array = []
    
  if katz_deli.length == 0
    puts "The line is currently empty."
  else
    count = 0
    while count < katz_deli.length
      idx_plus_one = count + 1
      line_array.push(idx_plus_one.to_s + " " + #{katz_deli[count]})
      count += 1
    end
    
    puts "The line is currently: #{line_array.join(" ")}"
    
  end
end
{% endhighlight %}

<p>The <i>all?</i>, <i>none?</i>, or <i>any?</i> methods are enumerables that return whether the element in its receiver is truthy or falsey based on a given block of code:</p>

{% highlight ruby %}
def none_even?(arr)
  # arr.all? {|int| int.odd?} is equivalent
  arr.none? {|int| int.even?}
end

def any_even?(arr)
  arr.any? {|int| int.even?}
end
{% endhighlight %}

<h4>The <i>reduce</i> or <i>inject</i> enumerable can be invoked in three ways. Whenever you find yourself setting a variable you reference throughout an iteration, consider using reduce to simplify your code.</h4>

<p><b>1.</b> With one binary argument (:+ or :lcm, for example):</p>

{% highlight ruby %}
[1, 2, 3].reduce(:+) #=> 6
# this is analogous to: 
def my_sum(arr)
    accumulator = arr.first # store first element as accumulator

    arr.each_index do |idx|
      next if idx == 0 # skip first element: it's already the accumulator 

      #By calling next, you can tell the current block iteration to halt exactly where it is, and tell the parent loop to continue on to the next iteration. 

      accumulator += arr[idx] # increment accumulator by current element
    end

    accumulator
  end
{% endhighlight %}

<p><b>2.</b> With a block (with an accumulator and the current element) and <b>without</b> an argument; Invoking reduce with a block gives greater control over how to reduce the receiver. One isn't limited to binary methods or operations.</p> 

{% highlight ruby %}
# These two invocations of reduce are functionally equivalent:
[1, 2, 3].reduce(:+)
[1, 2, 3].reduce {|acc, el| acc + el} # It returns acc when no elements remain.
# Example 2
def sum_first_and_odds(arr)
  arr.reduce do |acc, el|
    if el.odd?
      acc + el
    else
      # this else statement is necessary because otherwise the return value of
      # the block would be nil if the element is even. Thus the interpreter
      # would reassign acc to nil.
      acc
    end
  end
end
# The accumulator is simply a variable available throughout the iteration that's reassigned after each iteration.
# Example 3
# OLD SOLUTION
def longest_word(str)
  words = str.split
  longest_word = ""

  words.each do |word|
    if word.length > longest_word.length
      longest_word = word
    end
  end

  longest_word
end
# REDUCED EXCELLENCE
def longest_word(str)
  str.split.reduce do |longest, word|
    if word.length > longest.length
      word
    else
      longest
    end
  end
end

sum_first_and_odds([1, 2, 4, 5]) #=> 6
{% endhighlight %}

<p><b>3.</b> With a block and with one argument that's the initial accumulator; the block has two parameters: an accumulator and the current element. What about when we want to use a counter or a result array as the accumulator? The first element wouldn't suffice in most cases: </p>

{% highlight ruby %}
# There are two differences between invoking reduce with an argument and a block versus with only a block:
# 1. The interpreter initially assigns the accumulator to the given argument.
# 2. The interpreter iterates through the entire receiver, i.e., it does not skip the first element.
# OLD SOLUTION 1
def e_words(str)
  words = str.split
  count = 0

  words.each do |word|
    count += 1 if word[-1] == "e"
  end

  count
end
# REDUCED EXCELLENCE 1
def longest_word(str)
  str.split.reduce do |longest, word|
    if word.length > longest.length
      word
    else
      longest
    end
  end
end
def e_words(str)
  str.split.reduce(0) do |count, word|
    if word[-1] == "e"
      count + 1
    else
      count # return existing count from block so count isn't reassigned to nil
    end
  end
end
# Using reduce with an initial accumulator reduces defining a counter variable and iterating through a collection to a single method invocation.
# OLD SOLUTION 2
2def boolean_to_binary(arr)
  binary = ""

  arr.each do |boolean|
    if boolean
      binary += "1"
    else
      binary += "0"
    end
  end

  binary
end
# REDUCED EXCELLENCE 2
def boolean_to_binary(arr)
  arr.reduce("") do |str, boolean|
    if boolean
      str + "1"
    else
      str + "0"
    end
  end
end
# OLD SOLUTION 3
def factors(num)
  factors = []
  (1..num).each do |i|
    if num % i == 0
      factors << i
    end
  end
  factors
end
# REDUCED EXCELLENCE 3
def factors(num)
  (1..num).reduce([]) do |factors, i|
    if num % i == 0
      factors << i
    else
      factors
    end
  end
end

{% endhighlight %}
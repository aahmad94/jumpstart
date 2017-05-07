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
def line(katz_deli)
  
  line_array = [] # want to create a list that displays the current people in the katz_deli line with their respective place in line
    
  if katz_deli.length == 0 # want to consider if there's any exceptions we might want to consider
    puts "The line is currently empty."
  else 
    katz_deli.each.with_index(1) do |name, index| # now we can append our line_array with the people in line and their respective place in line 
      line_array.push("#{index}. #{name}") # the each method accepts a block of code (between 'do' and 'end' that runs that block of code for every element in the list we're accessing; alternatively we could use a while loop and a global counter variable that starts at 0 and continues as long as it is less than katz_deli.length and for each block iteration, append the line_array with: { (count + 1).to_s + " " + katz_deli[count] }.  
    end
    puts "The line is currently: #{line_array.join(" ")}" # this line will return the elements in line_array as a string
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
def boolean_to_binary(arr)
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


<h4>Glossary</h4>

<p><b>all?</b> - Passes each element in its receiver to a given block; returns true if the block never returns a falsey value (otherwise it returns false).<br>
<b>any?</b> - Passes each element in its receiver to a given block; returns true if the block ever returns a truthy value (otherwise it returns false).<br>
<b>count</b> - With no arguments: returns the number of elements in its receiver; with one argument: returns the number of elements in its receiver equal to its argument; with a block: returns the number of items in its receiver that, when passed to the block, return a truthy value.<br>
<b>each_with_index</b> - Calls the given block with two arguments--the item and the item's index--once for each element in the method's receiver.<br>
<b>map</b> - Returns a new array that's the result of executing its given block once for each element in its receiver.<br>
<b>none?</b> - Passes each element in its receiver to a given block; returns true if the block never returns a truthy value (otherwise it returns false).<br>
<b>reduce</b> - Combines all elements of its receiver by applying a binary operation, specified by a block or a symbol that names a method or operator; synonymous with inject.<br>
<b>reject</b> - Returns a collection containing all the elements in its receiver for which the given block returns a falsey value.<br>
<b>select</b> - Returns a collection containing all the elements in its receiver for which the given block returns a truthy value.<br>
<b>sort_by</b> - Sorts its receiver by the return values of its elements when they are passed to the given block and returns an array in that order.<br>
<b>with_index</b> - A chainable method that allows the block given to map or each_char to receive indices as well as receiver elements.</p>
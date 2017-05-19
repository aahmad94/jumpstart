---
layout: post
title: jumpstart step 1 05/19 hw: two_sum
date: 2017-05-19 03:00:00
---
<h3>Define a method, #two_sum, that accepts an array and a target sum (integer) as arguments.</h3>

<p>The method should return true if any two integers in the array sum to the target. Otherwise, it should return false. Assume the array will only contain integers.</p>

{% highlight ruby %}
def two_sum(array, target)
  
  o_count = 0 
  while o_count < array.length 
  
    i_count = o_count + 1
    while i_count < array.length 
    
      test_sum = array[o_count] + array[i_count]
      
      if test_sum == target
        return true
      end
      
      i_count += 1
    end
    o_count += 1
  end
  false
end
{% endhighlight %}


<p>The explicit return is necessary when you want to break out and stop the rest of the method execution immediately.</p>

<p>Another way to solve it is using enumerables; the <i>each</i> method takes the place of the outer loop and our while loop counter is set by identifying the index of the element we're using to make test summations.</p>

{% highlight ruby %}
def two_sum(array, target)
  array.each do |integer|
    count = array.index(integer)
    while count < (array.length - 1)
      all_other_integers = array[count + 1].to_i
      test_sum = integer + all_other_integers
      if test_sum == target
        return true
      end
      count += 1
    end
  end
  false
end
{% endhighlight %}

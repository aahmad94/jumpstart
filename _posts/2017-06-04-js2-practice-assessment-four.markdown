---
layout: post
title: jumpstart 2 practice assessment 4 and thoughts on helper methods
date: 2017-06-04 11:08:00
---

{% highlight ruby %}
def substring?(long_string, short_string)
  long_string.include?(short_string)
end
{% endhighlight %}

<h4>test. Write a method that, given a string, returns a hash in which each key is a character in the string and each value is an array of the first and last indices where that character occurs. If the character occurs only once, the array should contain a single index. Consider defining helper methods.</h4>

{% highlight ruby %}
# first_last_indices("banana") => {"b" => [0], "a" => [1, 5], "n" => [2, 4]}
def first_last_indices(str)

  chrs_indicies = Hash.new
  chrs = str.split('')
  uniq_chrs = chrs.uniq

  uniq_chrs.each do |uniq_chr|

    indicies = []
    chrs.each_index do |idx|
      if uniq_chr == chrs[idx]
        indicies << idx
      end
    end

    if indicies.length > 1
      chrs_indicies[uniq_chr] = [indicies[0], indicies[-1]]
    else
      chrs_indicies[uniq_chr] = [indicies[0]]
    end

  end
  chrs_indicies
end
{% endhighlight %}

<p>I didn't use a helper method and I usually don't for these things. I love the top-down approach and, for me at least, it's difficult to think clearly when fragmenting your thoughts on any particular solution. I do, however, think refactoring at the end is crucial when you're working on a larger scale for both readability and scalability. In this case, if I really wanted to, I could create a helper method to return <i>indicies</i> which I would use in the main method to assign a value for each unique character in a hash. </p>

<h4>Define a method that returns the number of times that two adjacent numbers in an array sum to n. Don't reuse numbers.</h4>

{% highlight ruby %}
# count_adjacent_sums([1, 5, 1, 3, 3], 6) => 2
def count_adjacent_sums(array, n)
  n_sums = []
  array.each_index do |idx|
    test_sum = array[idx] + array[idx+1] if idx != array.length - 1
    if test_sum == n && n_sums[-1] != idx
      n_sums << idx+1
    end
  end
  n_sums.length
end
{% endhighlight %}

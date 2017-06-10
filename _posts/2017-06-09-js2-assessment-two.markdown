---
layout: post
title: jumpstart 2 assessment 2
date: 2017-06-09 12:00:00
---

<h3>I passed the second assessment!</h3>

<p>I actually got stuck on the last one because I read the problem wrong; I though it wanted us to put all possible permutations of a string whereas it only wanted us to
 show the substrings present in the original string--this is why I need to not panic and slow down during these sort of timed assessments ¯\_(ツ)_/¯.</p>

{% highlight ruby %}
### Missing Numbers
#
# Given an array of unique integers ordered from least to greatest, write
# a method that returns an array of the integers that are needed to
# fill in the consecutive set.

def missing_numbers(nums)
  arr = []
  nums.each_index do |idx|
    if nums[idx] + 1 != nums[idx+1] && idx != nums.length - 1
      (nums[idx] + 1...nums[idx+1]).each { |num| arr << num }
    end
  end
  arr
end

puts "-------Missing Numbers-------"
puts missing_numbers([1, 3]) == [2]
puts missing_numbers([2, 3, 7]) == [4, 5, 6]
puts missing_numbers([-5, -1, 0, 3]) == [-4, -3, -2, 1, 2]
puts missing_numbers([0, 5]) == [1, 2, 3, 4]



### Titleize
#
# Write a method that capitalizes each word in a string like a book title.
# Do not capitalize words like 'a', 'and', 'of', 'over' or 'the'.

def titleize(title)
  result = []
  words = title.split(" ")
  special = %w[a and of over the]
  words.each_index do |idx|
    if !special.include?(words[idx]) || idx == 0
      words[idx] = words[idx][0].upcase + words[idx][1..-1]
      result << words[idx]
    else
      result << words[idx]
    end
  end
  result.join(" ")
end

puts "-------Titleize-------"
puts titleize("basketball") == "Basketball"
puts titleize("stephen curry") == "Stephen Curry"
puts titleize("war and peace") == "War and Peace"
puts titleize("the bridge over the river kwai") == "The Bridge over the River Kwai"



# Unique in Order
#
# Implement the function unique_in_order which takes a string and
# returns an array of letters without any elements with the same value next
# to each other. Preserve the original order of elements.

def unique_in_order(string)
  result = []
  letters = string.split("")
  letters.each_index do |idx|
    if result[-1] != letters[idx]
      result << letters[idx]
    end
  end
  result
end

puts "-------Unique in Order-------"
puts unique_in_order('AAAABBBCCDAABBB') == ['A', 'B', 'C', 'D', 'A', 'B']
puts unique_in_order('BBBAAAACCDAABBB') == ['B', 'A', 'C', 'D', 'A', 'B']
puts unique_in_order('ABBCcAD') == ['A', 'B', 'C', 'c', 'A', 'D']
puts unique_in_order('aAa') == ['a', 'A', 'a']



# Substrings
#
# Return an array of all the substrings for a given string.
# Be sure that the returned array is sorted.

def substrings(str)
  result = []
  i = 0
  while i < str.length
    result << str[i]
    j = i + 1
    while j < str.length
      result << str[i..j]
      j+= 1
    end
  i+= 1
  end
  result.sort
end

puts "-------Substrings-------"
puts substrings("") == []
puts substrings("123") == ["1", "12", "123", "2", "23", "3"]
puts substrings("ruby") == ["b", "by", "r", "ru", "rub", "ruby", "u", "ub", "uby", "y"]
{% endhighlight %}

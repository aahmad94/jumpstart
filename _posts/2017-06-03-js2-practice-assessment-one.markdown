---
layout: post
title: jumpstart 2 practice assessment 1
date: 2017-06-03 14:00:00
---

{% highlight ruby %}
# Anagrams are two words with exactly the same letters. Order does not matter. Define a method that, given two strings, returns a boolean indicating whether they are anagrams.
def anagrams?(str1, str2)

  str1_chars_count = Hash.new(0)
  str1.split("").each {|chr| str1_chars_count[chr] += 1}

  str2_chars_count = Hash.new(0)
  str2.split("").each {|chr| str2_chars_count[chr] += 1}

  str1_chars_count.all? {|k,_| str1_chars_count[k] == str2_chars_count[k]}

end

# An isogram is a word of only non-repeating letters. Define a method that, given two isograms of the same length, returns an array of two elements indicating matches: the first number is the number of letters matched in both words at the same position, and the second is the number of letters matched in both words but not in the same position.
def isogram_matcher(isogram1, isogram2)
  match_position = 0
  match_chr_not_position = 0

  isogram1.split('').each.with_index do |chr1, idx1|
    isogram2.split('').each.with_index do |chr2, idx2|
      if chr1 == chr2 && idx1 == idx2
        match_position += 1
      elsif chr1 == chr2
        match_chr_not_position += 1
      end
    end
  end
  [match_position, match_chr_not_position]
end

# You have a panoramic view in front of you, but you only can take a picture of two landmarks at a time (your camera is small). You want to capture every pair of landmarks that are next to each other. Define a method that, given an array of landmarks, returns an array of every adjacent pair from left to right. Assume the panorama wraps around.
def panoramic_pairs(landmarks)
  result = []
  landmarks.each_index {|idx| (idx.to_i == landmarks.length - 1) ? result << [landmarks[idx], landmarks[0]] : result << [landmarks[idx], landmarks[idx+1]]}
  result
end

# Xbonacci
# Define a Xbonacci method that works similarly to the fibonacci sequence.
# The fibonacci sequence takes the previous two numbers in the sequence and adds
# them together to create the next number.
#
# First five fibonacci numbers = [1, 1, 2, 3, 5]
# The fourth fibonacci number (3) is the sum of the two numbers before it
# (1 and 2).
#
# In Xbonacci, the sum of the previous X numbers (instead of the previous 2 numbers)
# of the sequence becomes the next number in the sequence.
#
# The method will take two arguments: the starting sequence with X number of
# elements and an integer N. The method will return the first N elements in the given
# sequence.
#
# xbonacci([1, 1], 5) => [1, 1, 2, 3, 5]
# xbonacci([1, 1, 1, 1], 8) => [1, 1, 1, 1, 4, 7, 13, 25]
#
# X is the length of the starting sequence.
#
# number_of_xbonacci_numbers_to_return is the same as N.

def xbonacci(starting_sequence, number_of_xbonacci_numbers_to_return)
  init_len = starting_sequence.length
  until starting_sequence.length == number_of_xbonacci_numbers_to_return
    starting_sequence << starting_sequence[-init_len..-1].reduce(:+)
  end
  starting_sequence
end

require_relative "test.rb"
{% endhighlight %}

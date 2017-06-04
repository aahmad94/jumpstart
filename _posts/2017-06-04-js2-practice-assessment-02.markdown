---
layout: post
title: jumpstart 2 practice assessment 2
date: 2017-06-03 15:00:00
---

{% highlight ruby %}
# Given an array of words, remove all the occurrences of the letter 'a' in those words and return the resulting array.

def remove_letter_a(words)
  words.each_index do |word_idx|
    chrs_in_word = words[word_idx].split('')
    chrs_in_word.delete_if {|chr| chr == 'a'}
    words[word_idx] = chrs_in_word.join('')
  end
  words
end

# An abundant number is less than the sum of its divisors, not including itself. 12's divisors are 1, 2, 3, 4, 6, which sum to 16. xw16 > 12 so 12 is an abundant number.
# Write a method that, given a number, returns a boolean indicating whether that number is abundant.

def abundant?(num)
  divisors = Array.new
  num.times {|int| divisors << int if int != 0 && int != num  && num % int == 0}
  num < divisors.reduce(:+) ? true : false
end

# Return the greatest number that's a factor of both inputs.
# greatest_common_factor(6, 10) => 2
# greatest_common_factor(10, 15) => 5

def greatest_common_factor(first_number, second_number)
  first_number_factors = Array.new
  first_number.times {|int| first_number_factors << int if int != 0  && first_number % int == 0}
  second_number_factors = Array.new
  second_number.times {|int| second_number_factors << int if int != 0  && second_number % int == 0}
  common_factors = Array.new
  first_number_factors.each {|factor| common_factors << factor if second_number_factors.include?(factor)}
  second_number_factors.each {|factor| common_factors << factor if first_number_factors.include?(factor)}
  common_factors.max
end

# Write a method that, given a sentence without punctuation or capitalization, returns the word with the greatest number of repeated letters. Return the first word if there's a tie. It doesn't matter how often individual letters repeat, just that they repeat.
# word_with_most_repeats("I took the road less traveled and that has made all the difference") => "difference" because "difference" has two repeated letters, more than the other words.

def word_with_most_repeats(sentence)
  words = sentence.gsub(/\W/, " ").split(" ")
  word_idx = nil
  repeats = nil
  words.each_index do |idx|
    words[idx].split("").each do |chr|
      chr_repeats = words[idx].downcase.count(chr.downcase)
      if repeats == nil || chr_repeats > repeats
        repeats = chr_repeats
        word_idx = idx
      end
    end
  end
  words[word_idx]
end
{% endhighlight %}

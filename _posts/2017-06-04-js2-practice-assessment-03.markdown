---
layout: post
title: jumpstart 2 practice assessment 3
date: 2017-06-04 16:00:00
---

{% highlight ruby %}
# Define a method that accepts two arguments, a string and an integer. The method should return a copy of the string with the letter at the nth index removed.

def remove_nth_letter(string, n)
  chrs = string.split('')
  chrs.delete_at(n)
  chrs.join('')
end

# Define a method that chunks an array into a nested array of sub-arrays of length n. The last array may be of length less than n if the original array's size does not divide evenly by n.
# chunk([1,2,3,4,5], 2) => [[1,2], [3,4], [5]]

def chunk(array, n)
  result = []
  chunk = []
  array.each do |el|
    if chunk.length == n
      result << chunk
      chunk = []
    end
    chunk << el
  end
  result << chunk
end

# Define a method that multiplies the frequencies of the periods, commas, hyphens, semicolons, question marks, and exclamation points in a given string and returns the product. If any punctuation does not occur, don't include it in the product, i.e., don't multiply by zero!

def product_punctuation(str)
  punc = %w[. , - ; ? !]
  counts = punc.map {|symbol| str.count(symbol)}
  counts.delete_if {|count| count == 0 || count == nil}
  counts.reduce {|acc, count| acc * count}
end

# Translate a sentence into pig-latin! The first consonants go to the end of the word, then add "ay".

def pig_latin(sentence)
  piggified = []
  vowels = %w[a e i o u]
  words = sentence.gsub(/\W/, " ").split(" ")
  words.each do |word|

    first_vowel_idx = word.index(word.split('').select {|chr| chr if vowels.include?(chr)}[0])

    piggified << word[first_vowel_idx..-1] + word[0...first_vowel_idx] + "ay"

  end
  piggified.join(" ")
end
{% endhighlight %}

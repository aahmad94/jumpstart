---
layout: post
title: jumpstart 2 hw hipsterify a sentence
date: 2017-06-04 13:30:00
---

<h4>Define a method, #hipsterfy, that accepts a sentence (string) as an argument.
The method should return a new string with the last vowel removed from each word.</h4>

<p><strong>1.</strong> Using an array to track indices:</p>

{% highlight ruby %}
def hipsterfy(sentence)

  hipster_sentence = []

  words = sentence.gsub(/\W/, " ").split(" ")
  words.each do |word|

    chrs = word.split("")
    last_vowel_idx = nil
    chrs.each_index {|chr_idx| last_vowel_idx = chr_idx if 'aeiou'.include?(word[chr_idx].downcase)}

    chrs.delete_at(last_vowel_idx) if last_vowel_idx
    hipster_sentence << chrs.join("")

  end
  hipster_sentence.join(" ")
end
{% endhighlight %}

<p><strong>1.</strong> Using a hash to track indices:</p>

{% highlight ruby %}
def hipsterfy(sentence)
  hipster_sentence = []

  words = sentence.downcase.gsub(/\W/, ' ').split(' ')
  words.each do |word|
    vowels_pos = Hash.new

    chrs = word.split('')
    chrs.each.with_index {|chr, idx| vowels_pos[chr] = idx if 'aeiou'.include?(chr.downcase)}

    chrs.delete_at(vowels_pos.values.max) if vowels_pos.values.max
    hipster_sentence << chrs.join('')

  end
  hipster_sentence.join(' ')
end
{% endhighlight %}

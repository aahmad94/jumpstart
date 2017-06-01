---
layout: post
title: jumpstart step 2 hw even splitters
date: 2017-06-01 13:30:00
---
<h3>Define a method, #even_splitters, that accepts a string and returns an array of characters on which the string can be split to produce substrings of the same length.</h3>

<p>Don't count empty strings after the split.<br>

Here's an example for "banana":<br>

"banana".split("a") # => ["b", "n", "n"] (all elements same length)<br>
"banana".split("b") # => ["", anana"] (all elements same length - there's only<br>
one element "anana" because the empty string doesn't count)<br>
"banana".split("n") # => ["ba", "a", "a"] (all elements NOT same length)<br>

result: ["b", "a"].</p>

{% highlight ruby %}
def even_splitters(string)
  chrs = string.split("")
  chrs_split_evenly = chrs.select do |chr|
    split_at_chr = string.split(chr)
    if split_at_chr.all? {|seg| seg == "" || seg.length == split_at_chr[-1].length}
      chr
    end
  end
  chrs_split_evenly.uniq
end

# test cases
puts "-----Even Splitters----"
puts even_splitters("") == []
puts even_splitters("t") == ["t"]
puts even_splitters("jk") == ["j", "k"]
puts even_splitters("xoxo") == ["x", "o"]
puts even_splitters("banana") == ["b","a"]
puts even_splitters("mishmash") == ["m","h"]
{% endhighlight %}

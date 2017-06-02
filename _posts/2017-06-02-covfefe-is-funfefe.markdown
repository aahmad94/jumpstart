---
layout: post
title: covfefe is funfefe
date: 2017-06-02 13:00:00
---

<h4>Your are given a string. You must replace the word(s) coverage by covfefe, however, if you don't find the word coverage in the string, you must add it at the end of the string with a leading space.</h4>

{% highlight ruby %}
def covfefe(tweet)
  standardized_tweet = tweet.downcase.gsub(/\W/, " ")
  if !standardized_tweet.include?("coverage")
    tweet + " covfefe"
  else
    standardized_tweet.split(" ").map {|word| word == "coverage" ? "covfefe" : word}.join(" ")
  end
end
{% endhighlight %}

<h4>Twitter needs to implement this script into their platform ¯\_(ツ)_/¯</h4>

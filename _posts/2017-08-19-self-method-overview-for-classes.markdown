---
layout: post
title: self method overview for classes
date: 2017-08-03 18:00:00
---

<h4>We’ll explore a few simple rules that will clarify both the definition and the relationship of self in the context of its use in ruby classes.</h4>
<h4>
  <ol>
    <li>Use self when setting/getting instance attributes inside a class definition.</li>
    <li>Use self to denote a method within the class definition as a class method.</li>
    <li>Use self to reference the calling object within an instance method definition.</li>
  </ol>
</h4>

{% highlight ruby %}
class Hash

  # Write your own Hash#select method by monkey patching the Hash class. Your
  # Hash#my_select method should have the functionailty of Hash#select described
  # above. Do not use Hash#select in your method.

  def my_select
    truthy_key_value_hash = Hash.new
    self.each do |key, value|
      if yield(key, value)
        truthy_key_value_hash[key] = value
      end
    end
    truthy_key_value_hash
  end

end
{% endhighlight %}

{% highlight ruby %}
module Singable
  def sing
    "I'm #{self.name}, and I'm singing!"
  end
end

class Person
  include Singable
  attr_accessor:name

  def initialize(name)
    @name=name
  end
end

#========irbsession========

> Shane = Person.new("Shane")
  => #<Person:0x14ak3@name="Shane">
> Shane.sing
  =>"I'm Shane, and I'm singing!"
> sing
  => undefined local variable or method 'sing' for main:Object (NameError)
> Singable.sing
  => undefined method 'sing' for Singable:Module (NoMethodError)
{% endhighlight %}
    

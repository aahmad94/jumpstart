---
layout: post
title: self method overview for classes
date: 2017-08-03 18:00:00
---

<h4>We’ll explore a few simple rules that will clarify both the definition and the relationship of self in the context of its use in ruby classes.</h4>

<p><strong>1. </strong>Use self to reference the <i>calling object within an instance method definition.</i></p>
<p><strong>2. </strong>Use self when <i>setting/getting instance attributes inside a class definition.</i> Since the method is called by a Person object, self references that specific instance and the sing method reads the name attribute.</p>

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

<p><strong>3. </strong>Use self to <i>denote a method within the class definition as a class method.</i></p>

{% highlight ruby %}
class Foo
  def self.bar
    puts 'class method'
  end

  def baz
    puts 'instance method'
  end
end

Foo.bar # => "class method"
Foo.baz # => NoMethodError: undefined method ‘baz’ for Foo:Class

Foo.new.baz # => instance method
Foo.new.bar # => NoMethodError: undefined method ‘bar’ for #<Foo:0x1e820>
{% endhighlight %}

<p>Here's another example we use self to denote a method within the class definition as a class method along with a global class variable (denoted with '@@''). We first create the population() class method, and then update the initialize() method to increment the @@crowd class variable every time a new Person is created.</p>
<p>In irb, we created three new Person objects. We can clearly observe this when we execute the class method Person.population on the next line of the irb session. In the definition of the method, we prefix it with self in order to denote it as a class method, just like in Golden Rule 2: Use self to denote a method within the class definition as a class method. You can see that it won’t work as an instance method when we try to run the same method on the first Person object we created. </p>

{% highlight ruby %}
class Person

  include Singable
  attr_accessor :name

  @@crowd = 0

  def initialize(name)
    @name = name
    @@crowd += 1
  end

  def existential_crisis
    self
  end

  def self.population
    @@crowd
  end
end

# ======== irb session ========

> a = Person.new("Aaron")
  => #<Person:0x344h9@name="Aaron">
> b = Person.new("Bill")
  => #<Person:0x984k2@name="Bill">
> c = Person.new("Charlie")
  => #<Person:0x8fj83@name="Charlie">
> Person.population
  => 3
> a.population
  => undefined method `population' for #<Person:0x344h9@name="Aaron">
     (NoMethodError)
{% endhighlight %}

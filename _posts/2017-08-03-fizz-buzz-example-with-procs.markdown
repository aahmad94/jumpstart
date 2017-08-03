---
layout: post
title: fizzbuzz example with procs
date: 2017-08-03 15:00:00
---

<h4>Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz”.</h4>

{% highlight ruby %}
(1..100).map do |n|
  if (n % 15).zero?
    'FizzBuzz'
  elsif (n % 3).zero?
    'Fizz'
  elsif (n % 5).zero?
    'Buzz'
  else
    n.to_s
  end
end
{% endhighlight %}

<h4>Since making and calling procs are the only “actions” our program can perform, we can try implementing a number n with code that repeats the action of calling a proc n times.</h4>

{% highlight ruby %}
def one(proc, x)
  proc[x]
end

def two(proc, x)
  proc[proc[x]]
end

def three(proc, x)
  proc[proc[proc[x]]]
end

def zero(proc, x)
  x
end

{% endhighlight %}

<h4>All of these implementations can be translated into methodless representations; for example, we can replace the method #one with a proc which takes two arguments and then calls the first argument with the second one. They look like this:</h4>

{% highlight ruby %}
ZERO  = -> p { -> x {       x    } }
ONE   = -> p { -> x {     p[x]   } }
TWO   = -> p { -> x {   p[p[x]]  } }
THREE = -> p { -> x { p[p[p[x]]] } }
# For FizzBuzz we need the numbers five, fifteen and one hundred, which can all be implemented using the same technique:

FIVE    = -> p { -> x { p[p[p[p[p[x]]]]] } }
FIFTEEN = -> p { -> x { p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[x]]]]]]]]]]]]]]] } }
HUNDRED = -> p { -> x { p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[x]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]] } }
{% endhighlight %}

<p>We can convince ourselves that they really do represent numbers in the first place, as such a method is needed to translate these foreign representations of numbers into native Ruby values:</p>

{% highlight ruby %}
def to_integer(proc)
  proc[-> n { n + 1 }][0]
end

(to_integer(ONE)..to_integer(HUNDRED)).map do |n|
  if (n % to_integer(FIFTEEN)).zero?
    'FizzBuzz'
  elsif (n % to_integer(THREE)).zero?
    'Fizz'
  elsif (n % to_integer(FIVE)).zero?
    'Buzz'
  else
    n.to_s
  end
end
{% endhighlight %}

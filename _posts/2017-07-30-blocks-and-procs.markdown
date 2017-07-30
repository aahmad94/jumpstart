---
layout: post
title: blocks and procs
date: 2017-08-07 17:00:00
---
<h4>
Procs connect parts of the program together and make values flow to where they’re needed.
</h4>

{% highlight ruby %}
greeting = Proc.new { |name| "Hello #{name}" }
greeting.call("Adeel")
{% endhighlight %}

<h4>
When defining a method that has a proc as a parameter the proc (object) cannot access the other parameters for that method unless the proc explicitly calls that value when the method is defined:
</h4>

{% highlight ruby %}
proc_add_1 = Proc.new {|num| num + 1}
proc_add_2 = Proc.new {|num| num + 2}

def chain_blocks(start_val, proc1, proc2, &proc3)
 val1 = proc1.call(start_val)
 val2 = proc2.call(val1)
 val3 = proc3.call(val2)
 val3
end

chain_blocks(1, proc_add_1, proc_add_2) { |num| num + 3 }

# with yield shorthand

def chain_blocks(start_val, proc1, proc2)
 val1 = proc1.call(start_val)
 val2 = proc2.call(val1)
 yield(val2)
end

chain_blocks(1, proc_add_1, proc_add_2) { |num| num + 3 }
{% endhighlight %}

<h4>
Ruby gives us a shortcut for when passing blocks with a single argument. When #to_proc is called on a symbol, we get back a Proc object that calls the to_proc method with the same name as the symbol as a parameter:
</h4>

{% highlight ruby %}
["a", "b", "c"].map { |s| s.upcase }
[1, 2, 5].select { |i| i.odd? }

["a", "b", "c"].map(&:upcase)
[1, 2, 5].select(&:odd?)
{% endhighlight %}

<h4>
What are lambdas and how are they different from procs?
</h4>

<h4>
<strong>1.</strong> When a lambda expects an argument, you need to pass those arguments or an exception will be thrown. However, in the case of the Proc, if the argument is not passed it automatically defaults to nil.
</h4>

{% highlight ruby %}
# there are two lambda notation:
lambda = -> (name) { puts "hello #{name}" } # lambda literal or dash rocket notation
lambda = lambda { |name| puts "Hello #{name}" } # block notation, this expression is equivalent to the one in the line above
lambda.call("John") # => Hello John
lambda.call # => ArgumentError

# here's the equivalent expression using a proc:

not_lambda = Proc.new { |name| puts "Hello #{name}" }
not_lambda.call("John") # => Hello John
not_lambda.call # => Hello
{% endhighlight %}

<h4>
<strong>2.</strong> <i>When a lambda encounters a return statement it will return execution to the enclosing method. However, when a Proc encounters a return statement it will jump out of itself, as well as the enclosing method.</i>
</h4>

{% highlight ruby %}
def lambda_method
 -> () { return "I was called from inside the lambda"}.call
 return "I was called from after the lambda"
end

puts lambda_method # => "I was called from after the lambda"

def proc_method
 Proc.new { return "I was called from inside the proc"}.call
 return "I was called from after the proc"
end

puts proc_method # => "I was called from inside the proc"
{% endhighlight %}

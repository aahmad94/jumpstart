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
greeting.call("Adeel") # => "Hello Adeel"
{% endhighlight %}

<h4>
When defining a method that has a proc as a parameter the proc (object) cannot access the other parameters for that method unless the proc explicitly calls that value when the method is defined:
</h4>

{% highlight ruby %}
proc_add_1 = Proc.new { |num| num + 1 }
proc_add_2 = Proc.new { |num| num + 2 }

def chain_blocks(start_val, proc1, proc2, &proc3)
 val1 = proc1.call(start_val)
 val2 = proc2.call(val1)
 val3 = proc3.call(val2)
 val3
end

chain_blocks(1, proc_add_1, proc_add_2) { |num| num + 3 } # => 7

# with yield shorthand

def chain_blocks(start_val, proc1, proc2)
 val1 = proc1.call(start_val)
 val2 = proc2.call(val1)
 yield(val2)
end

chain_blocks(1, proc_add_1, proc_add_2) { |num| num + 3 } # => 7
{% endhighlight %}

<h4>
Ruby gives us a shortcut for when passing blocks with a single argument. When #to_proc or, when applicable, the shorthand #& are called on a symbol, we get back a block that calls the method with the same name as the symbol:
</h4>

{% highlight ruby %}
["a", "b", "c"].map { |s| s.upcase } # => ["A", "B", "C"]
[1, 2, 5].select { |i| i.odd? } # => [true, false, true]

# the below expressions are equivalent:

["a", "b", "c"].map(&:upcase) # => ["A", "B", "C"]
[1, 2, 5].select(&:odd?) # => [true, false, true]

# with #to_proc

class Array
  def first_and_last
    [self.first, self.last]
  end
end

class String
  def first_and_last
    [self[0], self[-1]]
  end
end

symbolic_proc = :first_and_last.to_proc #=> #<Proc:0x007feb749b0070>
symbolic_proc.call([1,2,3]) #=> [1, 3]
symbolic_proc.call("ABCD") #=> ["A", "D"]
["Hello", "Goodbye"].map(&:first_and_last) # => [["H", "o"], ["G", "e"]]
{% endhighlight %}

<h4>
The & can be tricky because it does several things:<br>
<ol>
  <li>Converts blocks to procs</li>
  <li>Converts method names (passed as symbols) to procs</li>
  <li>Converts procs to blocks</li>
</ol>
Assume we have a method my_sort! that takes a block argument, like this:
</h4>

{% highlight ruby %}
animals = ['cats', 'dog', 'badgers']
animals.my_sort! do |animal1, animal2|
  animal1.length <=> animal2.length
end
animals # => ['dog', 'cats', 'badgers']
{% endhighlight %}

<h4>
  We can easily define a non-bang version of this method like so:
</h4>

{% highlight ruby %}
class Array
  def my_sort(&prc)
    self.dup.my_sort!(&prc)
  end
end
{% endhighlight %}

<h4>
  The two uses of & in the above example do different things: the first one calls #to_proc on a block argument, creating a first-class proc object that we can refer to with prc. But #my_sort! expects a block argument, not a proc, so we can't simply pass it prc. Instead, when we call #my_sort!, we use & again, but this time & means the opposite of what it meant in the previous line; now & is changing the proc back into a block.
</h4>

<h4>
What are lambdas and how are they different from procs?
</h4>

<h4>
<strong>1.</strong> When a lambda expects an argument, you need to pass those arguments or an exception will be thrown. However, in the case of the Proc, if the argument is not passed it automatically defaults to nil.
</h4>

{% highlight ruby %}
# there are two lambda notation:
my_lambda = -> (name) { puts "hello #{name}" } # lambda literal or dash rocket notation
my_lambda = lambda { |name| puts "Hello #{name}" } # block notation, this expression is equivalent to the one in the line above

my_lambda.call("Adeel") # => "Hello Adeel"
my_lambda.call # => ArgumentError

# here's the equivalent expression using a proc:

my_proc = Proc.new { |name| puts "Hello #{name}" }

my_proc.call("John") # => "Hello John"
my_proc.call # => "Hello"
{% endhighlight %}

<h4>
<strong>2.</strong> When a lambda encounters a return statement it will return execution to the enclosing method.<i> However, when a Proc encounters a return statement it will jump out of itself, as well as the enclosing method.</i>
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

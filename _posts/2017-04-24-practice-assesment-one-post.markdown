---
layout: post
title: ruby logic and control flow exercise 
date: 2017-04-25 12:30:00
---

<h3>I finished App Academy's practice assement one.</h3>

<p>I completed the first two problems fairly quickly (~10 min) but I had to play around with the magic numbers prompt, mostly because I didn't fully understand the prompt initially, which is why you shouldn't rush reading over the prompts! I thought it wanted us to list whether an integer, n, contained numbers that add to 7. When I realized that was wrong, I thought they wanted the method to return all integers that contain numbers that add up to 7 up to integer n. When I finally realized they wanted the method to spit out the first n number of integers that add to 7, I came to my final setup; App Academy's solutions follow mine below.</p>

{% highlight ruby %}
# Define a method that returns the sum of all the non-negative integers up to and including its argument.
# sum_to(3) => 6

def sum_to(int)
  
  value = 0
  
  counter = 1
  
  while counter <= int
  
    value += counter
    
    counter += 1
    
  end
  
  return value
  
end

# Define a method, #e_words(str), that accepts a string as an argument. Your method return the number of words in the string that end with the letter "e".
# e_words("tree") => 1
# e_words("Let be be finale of seem.") => 3

def e_words(str)
  
  result = 0
  
  el = str.split(" ")
  
  count = 0 
  
  while count < el.length
    
    if el[count][-1] == "e"
      
      result += 1
    
    end
      
    count += 1  
      
  end
  
  return result
  
end

# A magic number is a number whose digits, when added together, sum to 7, e.g., 34. Define a method that returns an array of the first n magic numbers. You may wish to write a helper method (magic_number?) that returns a boolean indicating whether a number is magic. This problem is harder than anything you'll receive on the coding assessment.
# magic_numbers(3) => [7, 16, 25]

def magic_number?(n)
  
  result = 0
  
  arr = n.to_s.split("")
  
  count = 0
  
  while count < arr.length
  
    result += arr[count].to_i 
    
    count += 1
    
  end
  
  if result == 7
    
    return true
    
  end
    
end

def magic_number?(n)
  
  result = 0
  
  arr = n.to_s.split("")
  
  count = 0
  
  while count < arr.length
  
    result += arr[count].to_i 
    
    count += 1
    
  end
  
  if result == 7
    
    return true
    
  end
    
end

def magic_numbers(n)
  
  magic_numbers = []
  
  num = 0

  while magic_numbers.length < n
  
    el = num += 1
    
    if magic_number?(el) == true
      
      magic_numbers << el
      
    end
    
  end
  
  return magic_numbers
  
end 
{% endhighlight %}

<h3>Solutions</h3>

{% highlight ruby %}
def sum_to(int)
  sum = 0
  current_num = 1

  while current_num < int + 1
    sum = sum + current_num
    current_num = current_num + 1
  end

  sum
end

def e_words(str)
  words = str.split
  count = 0

  i = 0
  while i < words.length
    word = words[i]

    if word[-1] == "e"
      count = count + 1
    end

    i = i + 1
  end

  count
end

def magic_number?(n)
  string_digits = n.to_s.split("")
  sum = 0

  i = 0
  while i < string_digits.length
    digit = string_digits[i]
    sum = sum + digit.to_i
    i = i + 1
  end

  sum == 7 # will either return true or false
end

def magic_numbers(n)
  magic_numbers_array = []
  current_num = 1

  while magic_numbers_array.length < n # keep incrementing until desired length (n) reached
    if magic_number?(current_num)
      magic_numbers_array << current_num
    end
    current_num = current_num + 1
  end

  magic_numbers_array
end
{% endhighlight %}
---
layout: post
title: ruby logic and control flow exercise 
date: 2017-04-25 0:30:00
---
<h3>I finished the logic and control flow exercise!</h3>
<p>The exercises were fairly straight forward, some required more thought than others. The methods actually do some pretty cool things and knowing how the method was put together just adds some extra satisfaction. I had to think about the prompt that required reverse engineering the <i>join</i> method for awhile but when I finally got it working it was well worth the patience! I posted App Academy's solutions in the bottom. I actually just looked at the solutions and realized the prompt never said we couldn't use the <i>join</i> method in the prompt requiring us to <i>join</i> elements in an array into a string... <strong>yikes</strong>. Well it should be interesting to look over the differences between our lines of code!</p>

<p>Edit:</p>
<p>I realized the <i>join</i> they used was the name of their variable, not a method!</p>

{% highlight ruby %}
# EASY 

# Return the middle character of a string. Return the middle two characters if the word is of even length, 
# e.g., middle_substring("middle") => "dd", 
# middle_substring("mid") => "i"

def middle_substring(str)

  first_middle_letter_index_even = string_length/2 - 1
  second_middle_letter_index_even = string_length/2
  middle_letter_index_odd = (str.length/2).floor # I didn't nead to use float; int/int = int

  if str.length % 2 == 0
    string_length = str.length
    return str[first_middle_letter_index_even .. second_middle_letter_index_even]
  else 
    return str[middle_letter_index_odd]
  end
end
  
# Return the number of vowels in a string.

  vowels_in_str = []
  vowels = ["a","A","e","E","i","I","o","O","u","U"]

  str_counter = 0
  while str_counter < str.length
  
    vowels_counter = 0
    while vowels_counter < vowels.length
    
      if test = str[str_counter].to_s == vowels[vowels_counter]
        vowels_in_str << str[str_counter]
      end
      
      vowels_counter += 1

    end

    str_counter += 1

  end

  return vowels_in_str.length

end

# MEDIUM 

# Return the argument with all its lowercase characters removed.

def destructive_uppercase(str)
  upcase_char_in_str = []
  
  counter = 0
  while counter < str.length
  
    if str[counter] == str.upcase[counter]
      upcase_char_in_str << str[counter]
    end

    counter += 1
  end
  return upcase_char_in_str.join
end


# Write a method that returns an array containing 
# all the elements of the argument array in reverse order.

def my_reverse(arr)
  return arr.reverse 
  # I don't think they wanted me to solve it this way... I could fill an empty result array via new_arr.upshift(arr[i]) and adjust the counter to i += 1.
end

# HARD 

# Write your own version of the join method. 
# Assume this method will always receive a 
# separator as the second argument.

def my_join(arr, separator)
  segments_str = ""

  counter = 0
  while counter < arr.length
    last_element = arr.length - 1
    if counter < last_element
      segments_str << arr[counter].to_s + separator.to_s
    else
      segments_str << arr[counter].to_s
    end
    counter += 1
  end
  return segments_str
end

# Return an array of integers from 1 to 30 (inclusive), except for each multiple of 3 replace the integer with "fizz", for each multiple of 5 replace the integer with "buzz", and for each multiple of both 3 and 5, replace the integer with "fizzbuzz".
def fizzbuzz
  
  arr = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30] 
  # I could have set the counter to 1 and the while loop to counter < 31 and shovel all the strings and integers in! 
  
  counter = 0
  while counter < arr.length
    
    if 3.gcd(arr[counter]) == 3 && 5.gcd(arr[counter]) == 5
      arr[counter] = "fizzbuzz"
    elsif 3.gcd(arr[counter]) == 3
      arr[counter] = "fizz"
    elsif 5.gcd(arr[counter]) == 5
      arr[counter] = "buzz"
    end
    
    counter += 1
  end
  return arr
end
{% endhighlight %}

<h3>Solutions</h3>

{% highlight ruby %}
# EASY

def middle_substring(str)
  mid = str.length / 2

  if str.length.odd?
    return str[mid]
  end

  #this code is reachable only if the argument is of even length
  str[mid-1..mid]
end

def num_vowels(str)
  vowel_count = 0
  vowels = ["a", "e", "i", "o", "u"]

  str_idx = 0
  while str_idx < str.length
    ch = str[str_idx]

    if vowels.include?(ch.downcase)
      vowel_count = vowel_count + 1
    end

    str_idx = str_idx + 1
  end

  vowel_count
end


# MEDIUM

def destructive_uppercase(str)
  new_str = ""
  str_idx = 0

  while str_idx < str.length
    ch = str[str_idx]

    if ch == ch.upcase
      new_str << ch
    end

    str_idx = str_idx + 1
  end

  new_str
end

def my_reverse(arr)
  reversed = []

  i = 0
  while i < arr.length
    el = arr[i]
    reversed.unshift(el)
    i = i + 1
  end

  reversed
end



# HARD

def my_join(arr, separator)
  join = ""
  idx = 0

  while idx < arr.length  
    join = join + arr[idx]

    if idx != arr.length - 1 # Don't want to add the separator after the last element
      join = join + separator

    end
    idx = idx + 1
  end

  join
end

def fizzbuzz
  fizzbuzzers = []
  int = 1

  while int < 31

    if int % 3 == 0 && int % 5 == 0
      fizzbuzzers << "fizzbuzz"
    elsif int % 5 == 0
      fizzbuzzers << "buzz"
    elsif int % 3 == 0
      fizzbuzzers << "fizz"
    else
      fizzbuzzers << int
    end

    int = int + 1
  end

  fizzbuzzers
end
{% endhighlight %}
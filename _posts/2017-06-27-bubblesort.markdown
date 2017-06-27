---
layout: post
title: bubble sort
date: 2017-06-27 19:00:00
---

<h3>Bubble sort is a simple comparison algorithm that iterates through each element in the list until it's sorted.</h3>

{% highlight ruby %}
# Write a function `bubble_sort(arr)` which will sort an array of
# integers using the "bubble sort"
# methodology. (http://en.wikipedia.org/wiki/Bubble_sort)
#
# Difficulty: 3/5

def bubble_sort(arr)
  count = 0
  while count < arr.length - 1
    (arr.length - 1).times do |idx|
      if arr[idx] > arr[idx + 1]
        arr[idx], arr[idx + 1] = arr[idx + 1], arr[idx1]
      end

    end
    count += 1
  end
  p arr
end

puts("\nTests for #bubble_sort")
puts("===============================================")
    puts "bubble_sort([]) == []: "  + (bubble_sort([]) == []).to_s
    puts "bubble_sort([1]) == [1]: "  + (bubble_sort([1]) == [1]).to_s
    puts "bubble_sort([5, 4, 3, 2, 1]) == [1, 2, 3, 4, 5]: "  + (bubble_sort([5, 4, 3, 2, 1]) == [1, 2, 3, 4, 5]).to_s
	puts "bubble_sort(['a','c','d','b']) == ['a','b','c','d']: " + (bubble_sort(['a','c','d','b']) == ['a','b','c','d']).to_s
puts("===============================================")
{% endhighlight %}

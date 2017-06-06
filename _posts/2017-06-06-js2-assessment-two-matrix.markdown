---
layout: post
title: jumpstart 2 assessment two transposing matrices
date: 2017-06-06 11:00:00
---

<h4><p>Transpose</p>

<p>You are given a n * n 2D matrix<br>
Example:<br>
Matrix = [[1, 2],
         [3, 4]]</p>

<p>Write a method that will transpose a matrix. The transpose of a matrix is
obtained by turning all of the columns of a matrix into the rows and vice versa.
So an element at index ij would be at index ji once tranposed.</p>

<p>For example:

transpose(Matrix) = [[1, 3],
    								[2, 4]]</p>

<p>You may assume a square matrix as input. Do not use `.transpose`</p></h4>

<p>My solution using a hash to store all elements at a particular index</p>
{% highlight ruby %}
def transpose(matrix)
  hash = {}
  (0...matrix.length).each do |num| # populate matrix with all possible indices as keys
    hash[num] = [] # value is a hash that we can push objects into
  end
  matrix.each do |arr|
    arr.each_index do |idx|
      hash[idx] << arr[idx] # push elements at array indices into hash value that has a corresponding index as it's key.
    end
  end
  hash.values # returns nested array of arrays that have the elements that belonged to a particular index
end
{% endhighlight %}

<p>These are the corresponding test cases that must pass as true:</p>
{% highlight ruby %}
puts "-------Transpose-------"
matrix_one = [[1, 2],
							[3, 4]]
matrix_two = [[1, 4, 7],
 							[2, 5, 8],
							[3, 6, 9]]
matrix_three = [[1, 2, 3, 4],
							 	[5, 6, 7, 8],
  						  [9, 10, 11, 12],
							  [13, 14, 15, 16]]
puts transpose(matrix_one) == [[1, 3],
 															 [2, 4]]
puts transpose(matrix_two) == [[1, 2, 3],
														   [4, 5, 6],
															 [7, 8, 9]]
puts transpose(matrix_three) == [[1, 5, 9, 13],
 																	 [2, 6, 10, 14],
																	 [3, 7, 11, 15],
																	 [4, 8, 12, 16]]
{% endhighlight %}

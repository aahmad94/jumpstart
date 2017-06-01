---
layout: post
title: jumpstart 2 hw cats in hats
date: 2017-05-31 18:00:00
---
<h4>
 <p>You have 100 cats.<br>
 Your rules are simple:</p>
 <p>whenever you visit a cat, you toggle it's hat status (if it
 already has a hat, you remove it... if it does not have a hat, you put one on).
 All of the cats start hat-less. You cycle through 100 rounds of visiting cats.
 In the 1st round, you visit every cat. In the second round, you visit every other cat.
 In the nth round, you visit every nth cat.. until the 100th round, in which you only
 visit the 100th cat.</p>
 <p>At the end of 100 rounds, which cats have hats?</p>
</h4>

<p><strong>1.</strong> Using a counter hash to track (via incrementing the count) whether cats' hats are on or off (evens ~ hats off, odds ~ hats on):</p>

{% highlight ruby %}
 def cats_in_hats

  litter = {}
  positions = (1..100).to_a
  positions.to_a.each {|num| litter[num] = 0} # populate hash with cat positions and no hats (even)

  positions.to_a.each do |visit| # track visit number (100 in total)
    count = visit
    while count <= positions.max  # hatting corresponds to adding 1 to an odd (unhatted) value
      litter[count] += 1
      count += visit # hatting frequency dependent on visit number
    end
  end

  hats_on_positions = [] # result array
  litter.each do |k, v|
    if v % 2 != 0 # if value is odd (hatted),  
      hats_on_positions << k # push k (cat position) to result array
    end
  end
  hats_on_positions
 end
{% endhighlight %}

 <p><strong>2.</strong> Modifying the "hat" (true/false) values in an array:</p>

{% highlight ruby %}
def cats_in_hats
  cats = Array.new(101) {false}
  cats.each.with_index(1) do |_, out_idx|
    count = out_idx
    while count <= 100
      if cats[count] == false
        cats[count] = true
      else
        cats[count] = false
      end
      count += out_idx
    end
  end
  cat_positions_with_hats = []
  cats.each.with_index do |hat_status, idx|
    cat_positions_with_hats << idx if hat_status == true
  end
  cat_positions_with_hats
end
{% endhighlight %}

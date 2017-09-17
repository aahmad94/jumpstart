---
layout: post
title:  mixing in modules
date: 2017-09-16 18:00:00
---

<p>Modules are a way of grouping together methods, classes, and constants. Modules give you two major benefits:</p>

<ol>
<li>Modules provide a namespace and prevent name clashes.</li>
<li>Modules implement the mixin facility.</li>
</ol>

{% highlight ruby %}
class Player

  def initialize(name, score)
    @name = name
    @score = score
  end

  attr_accessor :name, :score

  include Comparable

  def <=>(other)
    score <=> other.score
  end

end


class Game

  def initialize
    @players = []
  end

  attr_accessor :players

  # include Enumerable
  #
  # def each(&prc)
  #   players.each do |player|
  #     prc.call(player)
  #   end
  # end

  def enroll(player)
    players << player
  end

end

player1 = Player.new("Jo", 69)
player2 = Player.new("John", 80)

basketball = Game.new
basketball.enroll(player1)
basketball.enroll(player2)

p basketball.any? { |player| player.score > 80 } #=> true

{% endhighlight %}

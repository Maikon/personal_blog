---
title: Single Responsibility
layout: post
tag:
- apprenticeship
- SRP
- SOLID
category: blog
---

Part of the tasks that I have for this week, is to go through the first five chapters of *Practical Object-Oriented Design in Ruby* or [POODR](http://www.sandimetz.com/poodr/) for short, by Sandi Metz.

It's interesting for me because I started reading the book about halfway through my course at Makers Academy and at the time, Enrique (MA's teaching wizard) suggested that I should start reading it after I graduated and had gained more experience.

His advice back then is even more clear now as I've read the first two chapters again and they make a lot more sense now than what they did the first time around. Writing a considerable amount of Ruby code since then definitely had an impact on my understanding of the principles discussed.

In the first chapter Sandi gives a brief introduction to two different styles of programming; Object-Oriented and Procedural.
I could relate to that straight away since my TicTacToe [submission](https://github.com/Maikon/TicTacToe_Ruby) *suffers* from the latter, amongst other things.

As Sandi puts it:

"because you know the order of events you can write code to do each thing and then quite deliberately string things together, one after another."

Then looking at my code for starting a game of TTT:

{% highlight ruby %}
  #!/usr/bin/ruby

  require_relative 'board'
  require_relative 'human'
  require_relative 'computer'
  require_relative 'game_interface'
  require_relative 'game_sequence'

  human = Human.new
  board = Board.new
  cpu = Computer.new
  ui = GameInterface.new(board, human, cpu)
  ui.greet_player
  ui.set_human_attributes
  cpu.choose_mark_depending_on_opponent(human)
  sequence = GameSequence.new(ui)
  sequence.start

{% endhighlight %}

You can see how it fits the description. I have a bunch of events which I've put in a specific order and formed a procedure. A more OO way could look like this:

{% highlight ruby %}
# replacing everything from human downwards
Game.new(Board.new, Human.new.set_attributes, Computer.new.set_attributes).start
{% endhighlight %}

Of course that's assuming I made all the necessary changes in the structure of each class and improve the design in such a way that would allow me to instantiate a new game and start playing.

In chapter 2 Sandi expands on the first principle of [SOLID](https://en.wikipedia.org/wiki/Solid_(object-oriented_design)), the Single Responsibility Principle (SPR).

There were many parts which I found very interesting but the one that I need to keep reminding myself to apply, is the way with which you can determine if a class has a SR or not. The following code is from the book which will illustrate the technique:

{% highlight ruby %}
  class Gear
    attr_reader :chainring, :cog, :rim, :tire

    def initialize(chainring, cog, rim, tire)
      @chainring = chainring
    @cog = cog
    @rim = rim
    @tire = tire
    end

    def ratio
      chainring / cog.to_f
    end

    def gear_inches
      ratio * (rim + (tire *) 2)
    end
  end
{% endhighlight %}

One way to determine whether the above class has one responsibility is to try to describe it in a simple sentence. If you need to use the words **or** & **and** in the sentence then it means it's doing more than one thing. i.e "Gear does X" vs "Gear does X and a bit of O".
Another way which I hadn't thought of before until I read it in the book, is taking each method of that class and asking it as a question. The question should make sense when rephrased so for the above you could do the following:

**Gear, what is your ratio?** -> *Makes sense...*

**Gear, what are your gear_inces?** -> *Meh..*

**Gear, what is your tire(size)?**  -> (O_O)

In my `Board` class I have a method called `available_moves` so in this scenario asking *"Board, what are your available_moves?"*, makes sense. On the other hand having a method `choose_move` in a `GameInterface` class makes no sense in more than one way, for me at least.

Another important concept is how you should enforce single responsibility *everywhere*. Sandi expands on that by suggesting how you should extract extra responsibilities from methods. This wasn't completely new to me since I came across it at this [excellent talk](http://www.confreaks.com/videos/3181-rubyconfau2013-refactoring-from-good-to-great-a-live-coding-odyssey) by Ben Orenstein. One of the things to keep in mind is to remember separating the iteration from the action that's being performed on each element:

Book example:

{% highlight ruby %}
  def diameters
    wheels.collect do |wheel|
      wheel.rim + (wheel.tire * 2)
    end
  end
{% endhighlight %}

Separating the above into two methods; one performing the iteration, another for performing the action on an element:
{% highlight ruby %}
  def diameters
    wheels.collect { |wheel| diameter(wheel) }
  end

  def diameter(wheel)
    wheel.rim + (wheel.tire * 2)
  end
{% endhighlight %}

I'll start rewriting TicTacToe from tomorrow and apply some of the principles learned from the book so far.

I've taken a lot more notes from the chapter but one of my goals with this blog is to try and keep the posts short. I'll try my best to make them as clear and informative as possible and use code to explain each concept. If you have any comments, suggestions or in general call me out where I might be wrong, please do so.

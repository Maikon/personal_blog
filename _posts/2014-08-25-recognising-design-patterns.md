---
layout: post
title: "Recognising Design Patterns"
category: blog
tag:
- design patterns
- ruby
---

Since I started my journey with 8th Light, I've been immersed in best
practises and part of those practises, is learning and understanding
design patterns; knowing how to spot them, when to use them and when
not.

In my [last](http://maikon.github.io/2014/08/18/command-pattern.html)
blog post I wrote about the Command Pattern and even though I get the
general concept of it, it's still not evident how to spot it and when to
use it. An example I came across which caused some confusion is
something we use in one of our projects, a filtering queue.

To demonstrate an example, I shall use the ever so useful Marvel
Universe:

{% highlight ruby %}

class Character
  attr_reader :name, :type, :element
  attr_accessor :rating

  def initialize(name: name, type: type, element: element)
    @name = name
    @type = type
    @element = element
    @rating = 0
  end
end


class Hero < Character
end

class Villain < Character
end

class ElementFilter
  def execute(heroes, villain)
    heroes.select do |heroe|
      heroe.rating += 10 if heroe.element == villain.element
    end
  end
end

class TypeFilter
  def execute(heroes, villain)
    heroes.select do |heroe|
      heroe.rating += 10 if heroe.type == villain.type
    end
  end
end

class HeroeFinder
  def filter(filters, heroes, villain)
    list = filters.map do |filter|
      filter.execute(heroes, villain)
    end.flatten
    list.sort_by { |hero| hero.rating }.reverse.first
  end
end

colossus = Hero.new(name: 'Colossus', type: :mutant, element: :strength)
dr_strange = Hero.new(name: "Dr Strange", type: :sorcerer, element: :magic)
namor = Hero.new(name: "Namor", type: :mutant, element: :water)
heroes = [namor, colossus, dr_strange]
juggernaut = Villain.new(name: 'Juggernaut', type: :mutant, element: :strength)

hero_finder = HeroeFinder.new
filters = [TypeFilter.new, ElementFilter.new]
hero_finder.filter(filters, heroes, juggernaut)
#=> <Hero:0x007fc404b4d2d0 @element=:strength, @name="Colossus", @rating=20, @type=:mutant>
{% endhighlight %}

The intent of the Command Pattern is to: ***"Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations".*** The `ElementFilter` and `TypeFilter` are in this instance the ***command objects***.  The `HeroeFinder` acts as the client and takes a "queue" in the form of the `filters`. I could of put an instance variable with an array inside of it so that it would be more clear but I opted for removing the state instead and pass the queue directly in the `filter` method.

According to the definition above, this piece of code still conforms to the Command Pattern.  However patterns like [Chain of Responsibility](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern) add a level of confusion to the untrained eye (me). Despite the fact that clear differences exist, they can be tricky to see, especially if they're mixed with others. More training and practise is needed indeed.

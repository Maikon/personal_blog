---
layout: post
title: "Open-Closed Principle"
category: blog
tag:
- software design
- ocp
- solid
---

Today after lunch we were given a short talk by Daniel, a fellow apprentice, on the [Open-Closed Principle](http://en.wikipedia.org/wiki/Open/closed_principle), otherwise known as the 'O' in the [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) set of principles which act as a guideline for good OO code.

The OCP principle states that:

*Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.*

I'll try and demonstrate what that means using some past non-programming knowledge of mine:

{% highlight ruby %}
class RunningShoes
  ...
  def print_barefoot_list
    Nike.barefoot_list.each do |shoe|
      puts "The #{shoe.name} costs £#{shoe.price}"
    end
  end
end
{% endhighlight %}

The above method violates the OCP principle by having a concrete object (Nike) inside of it. By doing so it's not closed for modification since If I want to check the shoes of another brand, I'd have to go inside the method and change things around  (not open to extension).

Solution:
{% highlight ruby %}
class RunningShoes
  ...
  def print_barefoot_list(brand)
    brand.barefoot_list.each do |shoe|
      puts "The #{shoe.name} costs £#{shoe.price}"
    end
  end
end
{% endhighlight %}

This version on the other hand is. By using [duck typing](https://en.wikipedia.org/wiki/Duck_typing), I can get the list of any brand I wish as long as that class responds to the `barefoot_list` method accordingly.

One of the benefits of creating objects in such manner, is that the resulting objects are easier to move around since they don't depend on other concrete objects but rather on abstractions. If I wanted to move `RunningShoes` somewhere else, I'd have to take the `Nike` class with it. On the second scenario though I can move it without having to worry.

Below are some resources which I found useful and I still revisit as I'm trying to understand the general applications of the concept:

- [Open-Closed Principle (Thoughtbot Promo)](https://www.youtube.com/watch?v=FqSssh5GP_M)
- [The Open-Closed Principle (paper by 'Uncle Bob')](http://www.objectmentor.com/resources/articles/ocp.pdf)

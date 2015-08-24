---
layout: post
title: "Encapsulation."
category: post
---

It's only day 3 in my apprenticeship but I'm already aware of the amount of work that I need to go through, in order to reach the level that I'm expected to reach. It's exciting and alarming at the same time but in a good way. 

Because of that I've started experimenting with different productivity tips that I found in order to maximise my daily input and output. Since last night, I started making a short list of things (3-4 max) that I need to achieve during the next day and possible ways of trying to tackle them.

My list last night included 4 things which to me seemed like a good combo:

- Learn more [Encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(object-oriented_programming)) &  [Cohesion](https://en.wikipedia.org/wiki/Cohesion_%28computer_science%29)
- Start rewriting TTT
- Read one more chapter from POODR
- Write this fine piece of a post

Some of those terms are things which, I've heard or read about in the past but I am going through them again, and will continue to do so, until I have them internalised. Some of them I can recognise in other people's code and I have implemented them in mine too but it's one thing having these things travelling around your thoughts and it's a different thing being able to clearly explain what each is and does, every time someone asks you.

Cohesion is a principle which is also related in some ways to [Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_science)). I could cite the explanations from Wiki and such but I'll try and put these terms in my own words as that will help me understand them even better.

To my understanding and in a nutshell, Cohesion is about how closely related are the data within a class and the operations on that data. It's whether the methods within that class, serve a similar purpose. A good analogy to think about, is a standard pencil. It does one job and it does it well: leave a black mark. In this instance, the pencil is said to be *highly cohesive*. On the other hand, a pencil that *also* has an eraser attached to it, has *low cohesion* since it's operations serve two different purposes. A code example from the code I previously wrote for my TTT:

{% highlight ruby %}
  Class Board
    ....
    def winning_combinations
      rows + columns + diagonals
    end
    
    def rows
      grid.each_slice(3).to_a
    end
  
    def columns
      rows.transpose
    end
    ....
  end
{% endhighlight %}

All the methods are operating on the same objective (board's grid) and returning similar results (rows of the grid, columns of the grid).

The above to me seems to be also closely related to the *Single Responsibility Principle* as well. Do one thing, do it well.

Next up was Encapsulation. The concept is pretty simple. **Hide what you don't want others to know or have access to.** The idea is that you should only expose the code that is needed by other classes and parts of your program, and nothing more. The parts that you choose to expose are also referred to as the *public interface*.
Take the above code for example. Is there a need for other classes to know about rows and columns? Probably not. In Ruby to hide the above components of the Board object you would use the **private** keyword like so:

{% highlight ruby %}
  Class Board
    ....
    def winning_combinations
      rows + columns + diagonals
    end
  
    private
  
    def rows
      grid.each_slice(3).to_a
    end
  
    def columns
      rows.transpose
    end
    ....
  end
{% endhighlight %}
-------------------  
-------------------  
{% highlight ruby %}
  # before private
  Board.new.rows
  #=> [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
  # after private
  Board.new.rows
  #=> NoMethodError: private method...
{% endhighlight %}
  
By doing so you're restricting access to those methods, only allowing access from within the class itself. In this way no third parties can manipulate and corrupt the structure of your object. If someone was to try accessing the above two, they would simply get an error. There are other benefits as well but I'd need another post to explain each one, so that's for another time.

There's more things I'd like to write about (recognising dependencies, solutions etc) but I must remain disciplined in order to keep these posts short. All in all though it was a good day and apart from the usual rabbit hole I fall whenever I chase Vim related material, I did complete the task list. 

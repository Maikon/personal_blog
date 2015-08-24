---
layout: post
title: "The Hollywood Principle"
category: post
---

One of the things I've been learning more about recently is [callbacks](https://en.wikipedia.org/wiki/Callback_(computer_programming) and [events](https://en.wikipedia.org/wiki/Event_(computing). 

My QT GUI includes radio buttons which once they are clicked, they trigger appropriate events. I was talking with Christoph and saying how all of these are fairly new to me when he told me about the [**Hollywood Principle**](https://en.wikipedia.org/wiki/Hollywood_principle_(computer_programming).

The Hollywood Principle simply states: ***don't call us, we'll call you.*** A bit funny I know but it makes more sense when you see the application of the above. I'll give two examples from my Qt GUI. The first one deals with the cells of the grid. In my `Cell` class, which inherits from `Qt::Label`, I have a method called `mousePressEvent` like so:
{% highlight ruby %}
def mousePressEvent(_)
  return if @game.is_over?
  position = self.objectName.to_i
  if game.valid_move?(position)
    game.play_next_move(position)
  @parent.update_grid
end
{% endhighlight %}
  This method overrides the default Qt implementation of `mousePressEvent` and what it allows me to do is ***listen*** for a click on the cell. If a user clicks on the cell, I will get the name of it which I convert into an integer and mark the board. Then I send a message to the parent to update the grid. 

  In terms of the Hollywood Principle, `Qt` is Hollywood and it says "I deal with the clicking stuff and if somebody clicks, I'll let you know". Instead of me calling `Qt` and its method, `Qt` calls my program and method. It tells me to do something because an event occurred that I care about.

  The second example is fairly similar:
{% highlight ruby %}
  slots :human_vs_human

  def initialize
  ...
    @hvh_button  = Qt::RadioButton.new(self)
    connect(hvh_button, SIGNAL(:pressed), self, SLOT(:human_vs_human))
  end

  def human_vs_human
    # play the game
  end
{% endhighlight %}
  What happens here is I create a radio button and I connect it to an event. More specifically when the radio button is pressed it emits the signal `pressed`. `Qt` does the same thing as above; it watches out for that event, then informs me and calls the `human_vs_human` method. Again `Qt` tells me ***"don't call me, I'll call you"***.

  I had never heard of the Hollywood Principle prior to today but it's definitely a good way of thinking about this kind of programming. It reminds me that in certain situations it's much more preferable and suitable to allow the system take control and simply "feed it" with my program.

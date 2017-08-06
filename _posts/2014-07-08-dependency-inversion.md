---
layout: post
title: "The Dependency Inversion Principle"
category: blog
tag:
- solid
- dip
- software design
---

One of the tasks I had for this week was to extract my tictactoe core functionality into a gem. There were three components that made up the core of my game: `Board`, `Computer` and `Game`.

Sorting out the first two went smoothly, however when I went to extract the `Game` section, I realised I had violated one of the SOLID principles. In fact the specific violation indicated issues related to more than just one of the principles, which shows how closely related the SOLID principles are.

The first violation was that of the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) which states the following:

- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions.

To show how this affected the `Game` class let's look at the constructor:

{% highlight ruby %}
def initialize(display = CliDisplay.new, board = Board.new, computer = Computer.new)
  @display = display
  @board = board
  @computer = computer
end
{% endhighlight %}

I'm setting the defaults of `display`, `board` and `computer` to their equivalent classes. At the time that I was putting this together, I only had to build a command line interface so I thought that injecting the display into the game made sense. Now though, since I'm trying to create a gem which could then be used with other interfaces such as a GUI or Web interface, the above code won't be suitable for a number of reasons.

First, the command line interface is not needed. Second, not only will building a different interface require me to implement the names of the methods that `CliDisplay` already has, an even bigger problem is that the flow of the game will vary depending on the interface used. It's fine using `while` loops and blocking the stream with `gets` as you wait for input on the command line, but these things won't be necessary with the other interfaces.

##### Solution

The first step I took was to delete the contents of `Game` class (source control has our back) and the equivalent tests. This is something I have learned to do more frequently whilst at 8th Light. Trying to fiddle with existing code in order to create something completely different feels weird and furthermore, you might end up writing code that you don't need.

I then reconstructed the `Game` class such that the `CliDisplay` and any reference to it was removed. The `Game` class now simply provide an interface which any other module could use. Below is the new constructor with some of the other methods:

{% highlight ruby %}
def initialize(board = Board.new, computer = Computer.new)
  @board = board
  @computer = computer
end

def play_next_move(input)
  board.mark_position(input, board.current_mark)
end

def computer_makes_move
  computer.choose_mark(board)
  computer.make_move(board)
end

def reset
  board.reset
end
{% endhighlight %}

The next step was creating a `CliGame` class which takes a new game and a display as arguments and then executes the job that my old `Game` class used to do. For example, the method marking a position on the board based on user input is identical:

{% highlight ruby %}
# Old
def move_sequence
  move = receive_user_input
  while !move_valid?(move)
    display.invalid_move_message
    move = receive_user_input
  end
  board.mark_position(move, board.current_mark)
end

# New
def mark_position
  move = receive_user_input
  until game.valid_move?(move)
    display.invalid_move_message
    move = receive_user_input
  end
  game.play_next_move(move)
end
{% endhighlight %}

The `CliGame` now acts simply as a [plugin](https://en.wikipedia.org/wiki/Plug-in_(computing)), that is it extends the core functionality of the tictactoe game by providing a command line interface. Additional plugins for it like the ones mentioned above can be created without having to worry about the `Game` details anymore. I simply need to inject the `Game` and the rest of the implementation is up to that specific interface, whether that's a CLI or a GUI.

Since the original state of my game did not allow for extension, the [Open-Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle), which I described in this [post](http://maikon.github.io/2014/06/09/open-closed.html), was also violated, however I felt that the Dependency Inversion was the more pressing concern given the task at hand.

To sum it up the old code was attached to a concrete class and an idea that could change; it is more likely to change the interface than it is to change the game rules. Reversing the dependency to point to a more stable idea (game rules) allows for the extension of the program if there is a need to do so.

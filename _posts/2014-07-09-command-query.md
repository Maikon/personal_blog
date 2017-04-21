---
layout: post
title: "Command-Query Separation"
category: post
---

The Command-Query Separation was devised by [Bertrand Meyer](https://en.wikipedia.org/wiki/Bertrand_Meyer) and it states:

***Every method should either be a command that performs an action, or a query that returns data to the caller, but not both. In other words, asking a question should not change the answer.***

Essentially what the above means is that a command should do one thing. If they return a value then they should not have side-effects. If they alter the state then they should not return a value. An example from the TicTacToe game I was working on will help explaining the concept:

{% highlight ruby %}
class Game
  ...
  # adheres to CQS
  def play_next_move(input)
    board.mark_position(input, board.current_mark)
  end
  
  # adheres to CQS
  def current_mark
    board.current_mark
  end
  ...
end

class CliGame
  ...
  # violates CQS
  def receive_user_input
    display.show_board(game.board_grid)
    display.ask_for_move(game.current_mark)
  end
{% endhighlight %}

In the `play_next_move` method I alter the state of the system by making a move and changing the state of the board. In the `current_mark` method I only return a value, the current mark. In the `receive_user_input` however I do **both** which violates the principle. I first print the board on the terminal with `show_board` thus altering the state of the system. Then I return a value and to see it more clearly this is the implementation of the `ask_for_move` method:

{% highlight ruby %}
def ask_for_move(mark)
  @output.puts "It's #{mark}'s turn to choose a move from the available ones:"
  @input.gets.chomp.to_i
end
{% endhighlight %}

Even though the side-effect of the `receive_user_input` is obvious in this case, meaning you can see it on the output, other side-effects won't be and they can be a nightmare to fix hence why the CQS acts as a very good guideline when building a system.

##### Further reading:
- [Wikipedia page](https://en.wikipedia.org/wiki/Command-query_separation)
- [Martin Fowler's article](http://martinfowler.com/bliki/CommandQuerySeparation.html)

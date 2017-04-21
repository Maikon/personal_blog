---
layout: post
title: "Better Testing (part 1)"
category: post
---
Over the weekend I started redoing tic-tac-toe from scratch with the goal of having a human-vs-human game completed. Apart from it being a task, my main motivation for doing so was that in the last attempt I discovered that I needed less code than I initially thought I did. That was due to a combination of reasons (trying to predict the future, writing more code than necessary etc). Another motivation however was the 'pain' I experienced with testing when the time came to put the pieces together in the `Game` class.

In this first part I'll show an example of a method where I was setting the wrong expectations in the relevant tests. In my `Game` class I had a method called `receive_user_input` which would ask the user for a move and return the input in the correct format. It's worth noting that methods that are attached to `display` were using the standard streams ($stdin, $stdout) to output messages to the screen and receive input and which I tested separately in their own class. `receive_user_input` is essentially a wrapper of two of those methods:
{% highlight ruby %}
def receive_user_input
  display.print_board(board.grid)
  display.ask_for_move(board.current_mark)
end
{% endhighlight %}

What I thought was easy in terms of testing was not necessarily the best choice. The test I wrote for the same method in my previous version looked like this:
{% highlight ruby %}
it 'shows the board and asks user for move' do
  expect(game.display).to receive(:print_board)
  expect(game.display).to receive(:ask_for_move)
  game.receive_user_input
end
{% endhighlight %}

What I'm doing here is setting the expectations on the display, which in this case is an instance of the `CliDisplay`. The test would pass and it would fail if one of the method calls was missing from the `receive_user_input` method. One of the reasons for doing the above was that I did not want duplicate the tests I already had for those methods in their own class, `CliDisplay`. However there's a major problem with the above test.

By writing the test in that format, I coupled myself to the actual implementation of the `receive_user_input`; the test knew about the internals of the method. In other words it knew too much. If I was to simply change the name of the method to something else and replace it inside the method, the above test would break, even though the end result is still the same. A minor change is still a change but the test should not care about how the method works; only that is returning the correct result.

The new test I wrote looks like this:
{% highlight ruby %}
it 'receives input from the user for a move' do
  board = Board.new
  output = StringIO.new
  input = StringIO.new("1\n2\n")
  display = CliDisplay.new(output, input)
  game = Game.new(display, board)
  expect(game.receive_user_input).to eq 1
  expect(game.receive_user_input).to eq 2
end
{% endhighlight %}
    
Here the test is not concerned with the how the method is implemented at all. It only cares about the return value. I stubbed the response using the `StringIO` class so whenever `gets` was called it would take the first argument which is `1\n` and on the second call will get `2\n`. To clarify how that works, here's the implementation of the `ask_for_move` method:
{% highlight ruby %}
def ask_for_move(mark)
  @output.puts "It's #{mark}'s turn to choose a move from the available ones:"
  @input.gets.chomp.to_i
end
{% endhighlight %}
  
Now any changes I make to the internals of the method has no effect on the test, as long as it returns the same value.

On the second part tomorrow I'll show how I managed to test methods that include `while` and `until` loops, again without coupling myself to the implementation. If you have any feedback on improvements or mistakes I possibly made, feel free to comment below.

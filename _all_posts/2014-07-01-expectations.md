---
layout: post
title: "Expectations"
category: post
---

I personally correlate my ability to write good quality code with my ability to write good tests. For my code to improve, my testing must improve first. Writing the best possible tests in a given scenario will lead to writing the best possible code. Nothing more, nothing less.

A new slight obsession of mine is having 100% coverage. Now I understand that is not possible in all situations but that's still a worthy ideal to strive for. In the latest version of my TicTacToe game, I had a method that wasn't covered by the tests. When the game is over, the user gets asked if they want to play again and if so a new game is instantiated. This was essentially the problem. The code under question is below:
{% highlight ruby %}
def start_new_game
 display.clear_screen
 Game.new.start
end
{% endhighlight %}

In my other tests I would simply pass in a `FakeDisplay` like I did [here](http://maikon.github.io/2014/06/18/even-better-testing.html) and stub out the input and output. With the above however that wouldn't be possible since I'm instantiating a new `Game` within the method therefore I can pass anything to it unless I add parameters but that would make the for ugly code and tests.

As a result if I was to write a test for that method and run it, it would start a new game and block while waiting for input.

What I ended up doing was writing a new `Board` method which would reset the board and then start the game sequence. Before that though here's the test I wrote
{% highlight ruby %}
describe '#start_new_game' do
 it 'starts a new game' do
   board = TicTacToe::Board.new([1, 2, 3, 4, 'O', 'X', 'X', 'O', 'X'])
   display = TicTacToe::FakeDisplay.new([1, 4, 3])
   game = TicTacToe::Game.new(display, board)
   expect(board).to receive(:reset)
   game.start_new_game
 end
end
{% endhighlight %}

  Code to make it pass:
{% highlight ruby %}
def start_new_game
  display.clear_screen
  board.reset
  start
end
{% endhighlight %}

  In the test I set the expectation on board by saying that I expect it to receive the message `reset`. In general I don't like testing for the internals of a method but in this case the above serves a better purpose and that is the fact that it stubs out the return value of the method call. As soon as the `start_new_game` method reaches the call to `board.reset` the test will be satisfied and thus will not proceed executing the rest of the code which will start the loop of the game.

  I'm currently working on adding a GUI to my TicTacToe version so I will try and cover that next in a series of posts which will also help me further understand the whole process better.

---
layout: post
title: "Even Better Testing"
category: post
---

Yesterday we got to meet some more members of the 8th Light family that came over from Chicago and more specifically Micah, Angelique and Margaret. It was exciting meeting some of the people that started the company we're part of. 

After the introductions we got back to coding and I started redoing one of my katas, the roman numerals at which point Micah asked if I want to pair which was great. Since I was already half way through, Micah simply observed how I worked in order to complete it and then provided some valuable feedback. After that though we went through my code for TTT and we stopped at the tests for the `Game` class which I mentioned in my last blog post.

At the end of yesterday's post I said that the tests I wrote could definitely be improved upon. Micah showed me an alternative way which was superior to mine and I proceeded with refactoring. I left yesterday's post on so that the transition from one to the other is more clear.

I replaced the old tests with the new ones but to keep the post shorter I'll only show one here and compare it to the previous version. You can always check out my repository [here](https://github.com/Maikon/TicTacToe_new).

Before I had the following test:
{% highlight ruby %}
describe Game do
  let(:board)   { Board.new }
  let(:output)  { StringIO.new }
  let(:input)   { StringIO.new("1\n2\n") }
  let(:display) { CliDisplay.new(output, input) }
  let(:game)    { Game.new(display, board) }

  it 'does not alter the board if the input is invalid' do
    input = StringIO.new("x\n2\n")
    display = CliDisplay.new(output, input)
    game = Game.new(display, board)
    game.move_sequence
    expect(board.grid).to eq [1, 'X', 3, 4, 5, 6, 7, 8, 9]
  end
  ...
end
{% endhighlight %}
    
Which was replaced by this:
{% highlight ruby %}
describe Game do
  let(:board)   { Board.new }
  let(:display) { TicTacToe::FakeDisplay.new([1, 2]) }
  let(:game)    { Game.new(display, board) }

  it 'does not alter the board if the input is invalid' do
    display = TicTacToe::FakeDisplay.new([false, 2])
    game = Game.new(display, board)
    game.move_sequence
    expect(board.grid).to eq [1, 'X', 3, 4, 5, 6, 7, 8, 9]
  end
  ...
end
{% endhighlight %}

The first noticeable difference is at the top with only 3 components, one being the new arrival. I still bring in `Board` and I clearly need `Game` but in the place of the `CliDisplay` I now bring in `FakeDisplay` which is included in the `TicTacToe` module. The `FakeDisplay` looks like this:
{% highlight ruby %}
class FakeDisplay

  def initialize(moves)
    @moves = moves
  end

  # other methods
  
  def ask_for_move(mark)
    @moves.shift
  end
  
  # the rest of the methods
end
{% endhighlight %}
    
By doing the above, I've decoupled my tests even more by removing knowledge about the `CliDisplay`. Previously I had to bring in an extra dependency in the face of `CliDisplay` and on top of that I had to create instances of `StringIO` in which I would stub the response to various methods.
Now whenever the equivalent method is called it simply returns nothing so I don't need to pass a `StringIO` instance. 

In the above test, the `ask_for_move` method is called through `game.move_sequence` and I need to check that the board is modified accordingly depending on the input. In the constructor of `FakeDisplay` I have the `@moves` instance variable and at the test I pass an array to it with an invalid value and a valid one. Then in the fake `ask_for_move` method I take the array and by calling `shift` on it, I take out the first item when the initial call is made through `game.move_sequence` and because there's a `while` loop in the real implementation of `ask_for_move`, it asks again for a valid move. At that point it takes the next item from the fake `ask_for_move` which will be 2 and proceeds to modify the board.

I do the same when other methods of `CliDisplay` are called but in most cases no real response is needed so they're playing a similar role to the `StringIO` in the sense that they don't return anything and thus eliminating the need for using `StringIO` in this case.

This is just another example of how valuable it is for me or anyone really that's in the same position as me, to have people that can provide great feedback and advice as a result of compiled years of experience.

---
layout: post
title: "Even Better Testing"
---

Yesterday we got to meet some more members of the 8th Light family that came over from Chicago and more specifically Micah, Angelique and Margaret. It was exciting meeting some of the people that started the company we're part of. 

After the introductions we got back to coding and I started redoing one of my katas, the roman numerals at which point Micah asked if I want to pair which was great. Since I was already half way through, Micah simply observed how I worked in order to complete it and then provided some valuable feedback. After that though we went through my code for TTT and we stopped at the tests for the `Game` class which I mentioned in my last blog post.

At the end of yesterday's post I said that the tests I wrote could definitely be improved upon. Micah showed me an alternative way which was superior to mine and I proceeded with refactoring. I left yesterday's post on so that the transition from one to the other is more clear.

The new tests that replaced the old ones are the following:
{% highlight ruby %}
describe Game do
  let(:board)   { Board.new }
  let(:display) { TicTacToe::FakeDisplay.new([1, 2]) }
  let(:game)    { Game.new(display, board) }

  it 'receives input from the user for a move' do
    expect(game.receive_user_input).to eq 1
    expect(game.receive_user_input).to eq 2
  end

  it 'alters the board based on user input' do
    game.mark_board_position
    expect(board.grid).to eq ['X', 2, 3, 4, 5, 6, 7, 8, 9]
  end

  it 'returns true if the move is valid, false otherwise' do
    expect(game.move_valid?(1)).to eq true
    expect(game.move_valid?('x')).to eq false
  end

  it 'does not alter the board if the input is invalid' do
    display = TicTacToe::FakeDisplay.new([false, 2])
    game = Game.new(display, board)
    game.move_sequence
    expect(board.grid).to eq [1, 'X', 3, 4, 5, 6, 7, 8, 9]
  end

  it "plays the game until there's a winner" do
    board = Board.new(['X', 2, 3, 4, 'O', 6, 7, 8, 9])
    display = TicTacToe::FakeDisplay.new([2, 4, 3, "n\n"])
    game = Game.new(display, board)
    game.start
    expect(board.grid).to eq ['X', 'X', 'X', 'O', 'O', 6, 7, 8, 9]
  end

  it "plays the game until there's a draw" do
    board = Board.new([1, 2, 3, 4, 'O', 'X', 'X', 'O', 'X'])
    display = TicTacToe::FakeDisplay.new([1, 4, 3, 2, "n\n"])
    game = Game.new(display, board)
    game.start
    expect(board.grid).to eq ['O', 'X', 'O', 'X', 'O', 'X', 'X', 'O', 'X']
  end
{% endhighlight %}


By doing the above, I've decoupled my tests even more by removing knowledge about the `CliDisplay`. Previously I had to bring in an extra dependency in the face of `CliDisplay` and on top of that I had to create instances of `StringIO` in which I would stub the response to various methods.
Now whenever the equivalent method is called it simply returns nothing so I don't need to pass a `StringIO` instance. 

In the above test, the `ask_for_move` method is called through `game.move_sequence` and I need to check that the board is modified accordingly depending on the input. In the constructor of `FakeDisplay` I have the `@moves` instance variable and at the test I pass an array to it with an invalid value and a valid one. Then in the fake `ask_for_move` method I take the array and by calling `shift` on it, I take out the first item when the initial call is made through `game.move_sequence` and because there's a `while` loop in the real implementation of `ask_for_move`, it asks again for a valid move. At that point it takes the next item from the fake `ask_for_move` which will be 2 and proceeds to modify the board.

I do the same when other methods of `CliDisplay` are called but in most cases no real response is needed so they're playing a similar role to the `StringIO` in the sense that they don't return anything and thus eliminating the need for using `StringIO` in this case.

This is just another example of how valuable it is for me or anyone really that's in the same position as me, to have people that can provide great feedback and advice as a result of compiled years of experience.

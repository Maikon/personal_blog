---
layout: post
title: "Better Testing (part 2)"
category: post
---

Continuing from the post yesterday, I'll show another key aspect of my program where I was having a hard time testing it, which in this scenario means committing the ultimate sin and not testing it, and how I managed to test it efficiently this time around.

The major pain points where the two loops I had for controlling the flow of the game as well as checking for valid moves. The examples are below:
{% highlight ruby %}
class Game
  ...
  def move_sequence
    move = receive_user_input
    while !move_valid?(move)
      display.invalid_move_message
      move = receive_user_input
    end
    board.mark_position(move, board.current_mark)
  end
  
  def start
    display.greet_players
    until board.game_over?
      move_sequence
    end
    display.print_winning_message_for(board.last_move_mark) if board.winner?
    display.print_draw_message if board.available_moves.empty?
    play_again?
  end
  ...
end
{% endhighlight %}
    
The part that I struggled with the most was how to test that the while conditional would respond appropriately depending on the input and execute the correct thing inside of it. The same applied to the until loop.

Like I mentioned yesterday, the methods that are sent to `display`, are part of the class that deals with all the `puts` and `gets` in this situation.

Like I mentioned at the beginning I could't figure out how to test the above code without coupling it to the implementation, even though I knew how to stub out the response from input stream (`gets`). After talking with Jim, I was given the two following tips:

- Extract the condition the loop is checking into a method and test it
- Do the same with the internal part of the loop

Starting fresh and with the above in mind, the resulting tests looked like this:
{% highlight ruby %}
  let(:board)   { Board.new }
  let(:output)  { StringIO.new }
  let(:input)   { StringIO.new("1\n2\n") }
  let(:display) { CliDisplay.new(output, input) }
  let(:game)    { Game.new(display, board) }
  
  it 'returns true if the move is valid, false otherwise' do
    expect(game.move_valid?(1)).to eq true
    expect(game.move_valid?('x')).to eq false
  end

  it 'does not alter the board if the input is invalid' do
    input = StringIO.new("x\n2\n")
    display = CliDisplay.new(output, input)
    game = Game.new(display, board)
    game.move_sequence
    expect(board.grid).to eq [1, 'X', 3, 4, 5, 6, 7, 8, 9]
  end

  it "plays the game until there's a winner" do
    board = Board.new(['X', 2, 3, 4, 'O', 6, 7, 8, 9])
    input = StringIO.new("2\n4\n3\nn\n")
    display = CliDisplay.new(output, input)
    game = Game.new(display, board)
    game.start
    expect(board.grid).to eq ['X', 'X', 'X', 'O', 'O', 6, 7, 8, 9]
  end

  it "plays the game until there's a draw" do
    board = Board.new([1, 2, 3, 4, 'O', 'X', 'X', 'O', 'X'])
    input = StringIO.new("1\n4\n3\n2\nn\n")
    display = CliDisplay.new(output, input)
    game = Game.new(display, board)
    game.start
    expect(board.grid).to eq ['O', 'X', 'O', 'X', 'O', 'X', 'X', 'O', 'X']
  end

{% endhighlight %}
   
Let's tackle one at a time. The first test is pretty straightforward, checking whether the `move_valid?` method is working correctly and the implementation of the method is below:
{% highlight ruby %}
def move_valid?(move)
  board.available_moves.include?(move)
end
{% endhighlight %} 

Having in mind the mantra that the test should not know about the internals of the method is testing, the second test achieves just that. I feed two responses to the input stream where `gets` is called and the first time I pass it an invalid move (`x`) and the second time a valid move (`2`). Then I check that the board is in the right state. It might be a bit obvious but it's worth noting the order of the responses is important, as if I did it the other way around, running the test suite would halt until input was given to it.

The last two tests essentially test the same loop but for the two separate conditions that its `until` loop is checking against, a win and a draw. I pass a fixed board with some of the moves filled in already and I feed the input stream again with the right moves to simulate the scenarios as if two players where playing. Finally I check that the final board is in the right condition. The extra `n\n` in the end of the input represents a 'no' as on the last line of the method the `play_again?` method gets called which asks the player if he/she wants to play again so I prevent from starting another round and causing my tests running the game.

I'm certain that the above can be improved, but going from no tests to having reliable tests was a big improvement for me personally and taught me a lot about dealing with a scenario like the above. 

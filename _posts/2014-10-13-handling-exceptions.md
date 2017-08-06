---
layout: post
title: "Handling the right Exceptions"
category: blog
---

In the last couple of weeks I was tasked with re-arranging the structure of my ruby TTT game using some design patterns and architectural principles. The goal was to better understand these principles and where they might be useful as well as where they might not. During the process I also learned other things which don't necessarily fall under the above categories and it's one of those things that I will be writing about in this post.

#### The Right Exception

As it is obvious from the title post, the topic will be Exceptions and more specifically exceptions in Ruby and how to handle certain situations as well as avoiding some pitfalls which I fell in myself.

When I was writing the CLI version of my Game, I wanted to deal with the situation in which a user would exit the program abruptly with a `ctrl-c` for example. Instead of seeing the error and stacktrace, I wanted a simple "Thanks and goodbye" message to be dislayed. To do so however I first had to catch that interruption error and then display the message I want. For some reason when I was implementing this, I decided that rescuing `Exception` was a good idea (Yes, I can see your eyes rolling). My code looked something like this:

```ruby
module CLI
  class Game
    ...

    def start
	  begin
	    set_game_choice
	    play_game
	    show_results
	    play_another_round
	  rescue Exception
	    print_farewell_message
	  end
    end

	...
  end
end
```

Now the problem with this for anyone who doesn't know about Exceptions in Ruby can be seen in this [diagram](http://cl.ly/image/010f360p1H1L) taken from the ***Programming Ruby*** book. Since the `Exception` class is the "mother" of all exceptions, this meant I was setting up myself for a few issues along the road.

## Hiding API changes

Rescuing the parent class automatically meant that I was rescuing all of it's children too and one of the first instances that I noticed something was wrong was when I made changes to the core API that was used by `CLI::Game`.

For example the `play_game` method was using the `next_player_makes_move` method from the core `Game` class. The original method took a display argument like so `next_player_makes_move(display)`. When I changed the core gem and removed the display argument, I was expecting to see that change breaking at least couple of tests in the `CLI::Game` with the classic `ArgumentError`.

"Naturally" the tests were green and I was baffled. It might of been the time of the day that I was writing the code but I couldn't understand how it didn't complain about the change in the signature of the method. A glance at the diagram above though and the answer is fairly obvious. Since the display argument was not used at all in the body of the method, the code still worked fine and when the `ArgumentError` was raised, it was rescued by the block in the `start` method and life was back to "normal".

This meant that any future changes made that would or could break the
system, would go undetected and only show up when things went really
wrong. Hours or days could be lost trying to figure out the problem.

#### Hiding wrong test setup

Another result of the above exception handling was the fact that couple of my tests were broken but again they were showing up green. The issue was the setup code the tests were using. One of the tests can be seen below:

```ruby
it 'restarts the game' do
  core_game.board = winning_board_for_x
  allow(display).to receive(:another_round?).and_return(true)
  cli_game.start
  expect(core_game.board_grid).to eq [0, 1, 2, 3, 4, 5, 6, 7, 8]
end
```

The test essentially checked that when an end state is reached (win for x) then when a user selects to play again (stubbing the `another_round?` method on display), the board would be reset. This is the code inside the `play_another_round` method:

```ruby
def play_another_round
  if display.another_round?
    game.reset
	start
  else
	display.print_farewell_message
  end
end
```

As soon as I changed the exception I was catching to the appropriate `Interrupt` then the above test went in the red. The reason? After the board was reset, the `start` method was being called again so I was running the whole sequence again. At the top in the `set_game_choice` method the input stream was receiving the wrong stuff which resulted in a `NoMethodError` for the `Nil` class.

Again looking at the exceptions hierarchy `NoMethodError`'s main ancestor is `Exception`. The test was happy as soon as the board was reset so when the exception was thrown the rescue block caught it and everything was supposedly good even though my setup was wrong.

Having the wrong setup leads to the wrong tests and that inevitably leads to the wrong design. If the tests are not telling you the truth and exposing the pain of a wrong design then there's a much higher price to be paid later down the line.

#### Summary

I was lucky and caught the mistake I made early on. By making the changes to the public api of the gem, I was expecting the `CLI::Game` to break and when it didn't, it became obvious there was a problem. My program is fairly small compared to most programs out in the wild so I can only imagine the plethora of issues the above scenario would cause to a system much larger. When dealing with exceptions, catching the right one becomes priority number one. In Ruby if you are not sure about the different ones, have a look at the diagram I link to above. Another way is catching the exception and checking its ancestors:

```ruby
begin
# some code
rescue Interrupt => e
  puts e.class.ancestors
end

#=> SignalException
#=>	Exception
#=>	Object
#=>	Kernel
#=>	BasicObject
```

The article from which I found the diagram can be found [here](http://rubylearning.com/satishtalim/ruby_exceptions.html) and it covers exceptions at a great length.

These sort of mistakes are the ones that stick with you and remember. I'm
just glad it happened now in such a small scale and in an environment
where it's acceptable to fail.

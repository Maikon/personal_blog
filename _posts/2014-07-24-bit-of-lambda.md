---
layout: post
title: "A bit of Lamb(da)"
category: blog
---

Today I was working on my GUI and one of my goals was reducing the complexity of the method that was in control of which game was to be played, while at the same time making it more flexible to change. Because I had never used the Qt framework prior to this project and my knowledge in terms of event-based programming was limited, the first version of the `play` method looked like this:

{% highlight ruby %}
BUTTONS = { :hvh => :human_vs_human,
  :hvc => :human_vs_computer,
  :cvh => :computer_vs_human,
  :cvc => :computer_vs_computer
}

def play
  case @game_choice
  when BUTTONS[:hvh]
    update_grid
  when BUTTONS[:hvc]
    update_grid
    game.computer_makes_move if !game.is_over?
    update_grid
  when BUTTONS[:cvh]
    game.computer_makes_move
    update_grid
  when BUTTONS[:cvc]
    until game.is_over?
      game.computer_makes_move
      update_grid
    end
  end
end
{% endhighlight %}

Needless to say this is bad. A case statement can often be a sign of a smell and here that was indeed the case. This method has a bunch of conditionals and even a conditional within a conditional (until). It's hard to change and it definitely violates the OCP principle since if I wanted to extend it in any way, I'd have to open the method and modify it in order to add the new game type.

I discussed this with my mentor and he pointed out most of the above, suggesting that I remove the `computer_vs_computer` option, clean up the design and then add it again. After working with it for a bit and simplifying things more, I arrived at this:

{% highlight ruby %}
def play
  update_grid
  if @game_choice == BUTTONS[:hvc] || @game_choice == BUTTONS[:cvh]
    game.computer_makes_move
    update_grid
  end
end
{% endhighlight %}
Better but still that long if statement bothered me quite a bit. Then I turned to a tool Ruby provides yet I had never used until now, the `lambda`.

What I essentially did was to store each game type into it's own lambda, then set an instance variable to the chosen lambda and execute/call it when the time is right. The resulting code looked like this:

{% highlight ruby %}
def initiliaze
  ...
  @update_the_grid = Proc.new { update_grid }
  ...
end

def play
  @game_choice.call(@update_the_grid, @game)
end

human_vs_human_game = ->(obj, game) do
  obj.call
end

human_vs_computer_game = ->(obj, game) do
  obj.call
  game.computer_makes_move unless game.is_over?
  obj.call
end

computer_vs_human_game = ->(obj, game) do
  game.computer_makes_move
  obj.call
end

  GAME_TYPES = { :hvh => human_vs_human_game,
                 :hvc => human_vs_computer_game,
                 :cvh => computer_vs_human_game 
               }
{% endhighlight %}
So now when the button is pressed the appropriate method gets triggered which sets the `@game_choice` variable to the correct lambda and when the user clicks on a cell at the end of that method, it sends a message to the `play` method which in turn calls the lambda with the correct argument:

{% highlight ruby %}
def human_vs_computer
  @game_choice = GAME_TYPES[:hvc]
  update_layout
end
{% endhighlight %}

There's still plenty of work to be done but at least now I can add more types of games to it if need be and taking away the case statement and the until loop is definitely a win. I discovered a bunch of interesting things about `lambda` and `Proc` during the process which I will try and document in another post tomorrow.

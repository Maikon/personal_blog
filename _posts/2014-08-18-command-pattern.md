---
layout: post
title: "The Command Pattern."
category: post
---
The last couple of weeks I've been working on an internal Rails project
and to say that I've been learning a lot it would probably be an
understatement. One of the things I've noticed though is the frequent
use and emphasis on [Design
Patterns](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)).

One of the design patterns I've seen often is the [Command
Pattern](https://en.wikipedia.org/wiki/Command_pattern).

Definition: "The Command Pattern is a behavioural design pattern in
which an object is used to represent and encapsulate all the information
needed to call a method at a later time."

In Ruby a lot of patterns are not so easy to notice because of the
dynamic nature of the language. The "switch" example on the Wikipedia
page is an easy one to understand so I'll implement it in Ruby.

The "Invoker" class:

{% highlight ruby %}
class Switch
  def execute(command)
    command.execute
  end
end
{% endhighlight %}

The "Receiver" class:

{% highlight ruby %}
class Light
  def turn_on
    puts "Lights ON"
  end

  def turn_off
    puts "Lights OFF"
  end
end
{% endhighlight %}

The "Command" classes:

{% highlight ruby %}
class FlipUpCommand
  attr_reader :light

  def initialize(light)
    @light = light
  end

  def execute
    light.turn_on
  end
end

class FlipDownCommand
  attr_reader :light

  def initialize(light)
    @light = light
  end

  def execute
    light.turn_off
  end
end
{% endhighlight %}

The "Client" class

{% highlight ruby %}
class HouseLight
  def initialize(switch)
    @switch = switch
  end

  def switch(command)
    @switch.execute(command)
  end
end
{% endhighlight %}

Putting it together:

{% highlight ruby %}
house_light = HouseLight.new(Switch.new)
light = Light.new
house_light.switch(FlipUpCommand.new(light))
#=> Lights ON
house_light.switch(FlipDownCommand.new(light))
#=> Lights OFF
{% endhighlight %}

The "Invoker" class takes the command objects and is responsible for invoking them when needed. The "Command" objects take a "Receiver" object and invoke a method which is specific to that receiver. The "Client" is responsible for bringing those together and is generally where the decision is made on when and how a command is to be executed. Typically the Client object (`HouseLight`) passes the Command object (`FlipUpCommand`) to the Invoker object (`Switch`) which then executes the command.

The reason why in Ruby is harder to see this sort of pattern is because you can dump the "Invoker" object and let the magic of Duck Typing do the rest: 

{% highlight ruby %}
class GardenLight
  def initialize(switches)
    @switches = switches
  end

  def switch(command)
    @switches[command].execute
  end
end

light = Light.new
garden_light = GardenLight.new({ ON: FlipUpCommand.new(light), 
                                 OFF: FlipDownCommand.new(light) })
garden_light.switch(:ON)
#=> Lights ON
garden_light.switch(:OFF)
#=> Lights OFF
{% endhighlight %}

There are several benefits when applying this pattern.  For one testing becomes much easier since you can isolate each class and test it with ease since there's no real dependencies.  Also if you decide to use the light and switch elsewhere then it's pretty easy.  Creating extra actions or various switches that do different things should also be easy based on the above.

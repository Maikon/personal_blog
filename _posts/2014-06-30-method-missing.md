---
layout: post
title: "Method Missing: Friend or Foe?"
category: blog
tag:
- ruby
- metaprogramming
---

In Ruby when you send a message to an object, the object will look for the first method it will find in its path and execute it. If it doesn't it will simply throw a NoMethodError:

{% highlight ruby %}

class Person
  def name
    "Joe"
  end
end

Person.new.age?
#=> NoMethodError: undefined method "age?" for Person:Class

{% endhighlight %}

One of Ruby's interesting features is the `method_missing` method. Amongst the many things this method allows you to do is to define default behaviour when trying to call a method that doesn't exist:

{% highlight ruby %}
class Person
  def method_missing(m)
    puts "#{m}: no such method implemented in this class"
  end
end

Person.new.age?
#=> age?: no such method implemented in this class
{% endhighlight %}

The above can be very useful in providing meaningful feedback but it's also used extensively in [Metaprogramming](http://www.sitepoint.com/ruby-metaprogramming-part-i/).

Now you'll wonder why I included the word "Foe" in the title. Today I was working on my TicTacToe implementation with the goal of adding a GUI to it. I was using [Qt](http://qt-project.org/) and more specifically the [Ruby bindings](https://github.com/ryanmelt/qtbindings).

As I was working and experimenting with it I was constructing various layouts and adding widgets to them:

{% highlight ruby %}
require 'qt'

class GuiDisplay < Qt::Widget

  slots :remove_button

  def initialize
    super
    self.windowTitle = 'Tic Tac Toe'
    layout_setup
    add_button_one
  end

  private

  def layout_setup
    @layout = Qt::VBoxLayout.new(self)
    @buttons_layout = Qt::HBoxLayout.new
    @layout.add_layout(@buttons_layout)
  end

  def add_button_one
    @button_1 = Qt::PushButton.new(self)
    @button_1.text = 'Human Vs Human'
    connect(@button_1, SIGNAL(:clicked), self, SLOT(:remove_button))
    @buttons_layout.add_widget(@button_1)
  end
end
{% endhighlight %}

  I won't go into detail what each piece of the code does but the key is the line that starts with `connect`. There I'm essentially connecting `@button_1` to a slot containing the method `show_board` which would get triggered when the button gets clicked. This is the relevant method:

{% highlight ruby %}
  def remove_button
    @buttons_layout.remove_widget(@button_1)
  end
{% endhighlight %}

  Now the above works fine. My issues occurred when I was trying things out prior to figuring it out. Because Qt is written in C++ the methods are written in CamelCase so the `remove_widget` above looks like `removeWidget`. I appreciate the work that was put into providing this bindings in Ruby but it's obvious that the implementations are not consistent across the board as you can see by the `windowTitle` method in the constructor. My main issue though was with `method_missing`. Whenever I called a method on `@buttons_layout` such as `remove_widget` without any parameter, I was failing the test and getting a NoMethodError even though I could see that the method existed on the object by simply calling `@buttons_layout.methods`. After expressing my frustration to the rest of the guys, Daniel who had worked with the framework before suggest passing anything as an argument. I passed `nil` and indeed it work.

  I must of spent a good hour or more trying to understand the above because in my mind  a method that takes arguments and doesn't get passed any arguments, it returns an `ArgumentError`. A `NoMethodError` is the last thing I would be expecting since I can see that the method exist.

  I might be missing something or not know enough about the subject but surely the above does not seem right to me. I haven't looked into the source code of qtbindings yet but I would like to see the implementation that causes such a result. In the meantime I'll try and understand better the framework and hopefully avoid such pitfalls. Just like with anything `method_missing` is a great tool which if misused can cause major issues.

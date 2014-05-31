---
layout: post
title: "Managing Dependencies: Part 1"
---

It's the end of week 1 of my apprenticeship but only day 4 (monday was bank holiday) and it's been an exciting first week. Being surrounded by 3 very good developers has helped me learn an incredible amount of things. Pairing with each one of them helps me learn new things, as each of them have their own unique style and approach. Of course with Jim acting as our mentor, there's some common practise in the things we learn but I got some great feedback this week from both Daniel and Uku.

On this post I'm going to write a bit more about chapter 3 from Sandi's POODR book and more specifically the part about *managing dependencies*.

Whenever I write a piece of new code or I'm checking old pieces, I'm always on the lookout or worry of dependencies. Some of the most important principles in programming revolve around the management of dependencies and as Sandi puts it in her book ***"Object-Oriented Design is about managing dependencies."***

Some ways of recognising dependencies is checking whether any of the following conditions is true:

---
- If an object knows the name of another class:

{% highlight ruby %}
    class A
      ...
      def some_method
        Person.new('Joe').name
      end
      ...
    end
{% endhighlight %}

In the above `class A` knows explicitly about `class Person` therefore coupling itself to that class.

---

- if an object knows the name of a message it intends to send to someone other than self

{% highlight ruby %}
    class A
      ...
      def some_method
        Person.new('Joe').name
      end
      ...
    end
{% endhighlight %}

In this scenario the `class A` knows about a message that is not destined to itself (**name**) but to the `class Person`. Again this creates similar coupling to what you would get in the first scenario, as now if you were to change the method *name* in some way that would alter it's behaviour or even a simple name change, that piece of code in `class A` would break.

---

- if an object knows the arguments that a message requires
- if an object knows the order of the required arguments

{% highlight ruby %}
    class A
      ...
      def some_method
        Person.new('Joe', 26).name
      end
      ...
    end
    
    class Person
      attr_reader :name, :age
      
      def initialize(name, age)
        @name = name
        @age = age
      end
    end
{% endhighlight %}
Here `class A` knows that a `class Person` object *needs* two arguments, *name* and *age*. It also knows that they have to come in a certain order. The *name* argument must be passed first before the number, otherwise it changes everything, aka breaks.

Due to insufficient time to finish this post, part 2 with some solutions to these issues, will follow tomorrow.

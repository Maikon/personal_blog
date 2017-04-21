---
layout: post
title: "Polymorphism."
category: post
---
In today's post, I'll try and explain [Polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)). Wikipedia is generally pretty good at explaining programming concepts but sometimes it's not as friendly to beginners (me).

In a nutshell, Polymorphism gives you the ability to send the same message to different objects and get different results.

Two ways with which Polymorphism can be applied are:

- Inheritance
- Duck Typing
<br/>
<br/>


###### Inheritance example
{% highlight ruby %}
 class Duck
   def quack
     puts 'Duck quack here!'
   end
 end
 
 def Goose < Duck
   def quack
     puts 'Goose quack here!'
   end
 end
 
 Duck.new.quack
 #=> Duck quack here!
 
 Goose.new.quack
 #=> Goose quack here!
{% endhighlight %}

The `Goose` class inherits from the `Duck` class and we have two different objects that respond to the same message but return different results. In this scenario, the benefit might not be very clear but you can see below why this a useful concept.

###### Duck Typing
Often when people describe polymorphism or the general concept of it, they will say *"if it walks like a duck and it talks like a Duck, then treat it as a Duck"*.
{% highlight ruby %}
class Duck
    def quack
      puts 'Duck quack here!'
    end
  end
  
  def Goose
    def quack
      puts 'Goose quack here!'
    end
  end
  
  [Duck.new, Goose.new].each do |object|
    object.quack
  end
  #=> Duck quack here!
  #=> Goose quack here!
{% endhighlight %}

As I understand it, the message `quack` inside the each loop is *polymorphic*; it returns different forms(results). Sending `quack` to a `Duck` object will you get a different result(form) than sending it to a `Goose`. There's different explanations but for the code I've seen so far, the message is what would be described as polymorphic. 
Sandi Metz's explanation in POODR is:

> Polymorphism in OOP refers to the ability of many different objects to respond to the same message. Senders of the message need not care about the class of the receiver; receivers supply their own specific version of the behaviour. A single message thus has many (poly) forms (morphs).

and they usually depend on the language they're implemented. It's a subject that deserves further reading.

By applying Polymorphism certain things become much easier. The code that you would need to write in order to check for different types of objects, will be less as a result. Code can also be shared more easily between classes that need it, without risking coupling between them.

From the little research I've done so far, the disadvantages that polymorphism seems to have, are mostly related to design. For example it might not be the easiest thing to see at first, especially for a beginner like me. Abstracting things to a level that will pay off, comes with years of practise and experience. Abusing it can also cause many issues, especially when done with inheritance.

So far I'm pretty satisfied that I understand the concept a lot more since the first time I came across it back at Makers Academy. I remember thinking to myself what on earth a duck has to do with a programming concept but now I can see where that came from. On the other hand, ducks are [everywhere](https://en.wikipedia.org/wiki/Rubber_duck_debugging) it seems.

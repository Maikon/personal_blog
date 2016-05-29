---
layout: post
title: "Managing Dependencies."
category: post
---

It's the end of week 1 of my apprenticeship but only day 4 (monday was bank holiday) and it's been an exciting first week. Being surrounded by 3 very good developers has helped me learn an incredible amount of things. Pairing with each one of them helps me learn new things, as each of them have their own unique style and approach. Of course with Jim acting as our mentor, there's some common practise in the things we learn but I got some great feedback this week from both Daniel and Uku.

On this post I'm going to write a bit more about chapter 3 from Sandi's POODR book and more specifically the part about *managing dependencies*.

Whenever I write a piece of new code or I'm checking old pieces, I'm always on the lookout or worry of dependencies. Some of the most important principles in programming revolve around the management of dependencies and as Sandi puts it in her book ***"Object-Oriented Design is about managing dependencies."***

Some ways of recognising dependencies is checking whether any of the following conditions is true:

- If an object knows the name of another class:
- if an object knows the name of a message it intends to send to someone other than self
<br/>
{% highlight ruby %}
 class Company
   ...
   def set_employee_record
     details = Person.new('Joe').get_personal_information
     # some code
   end
   ...
 end
{% endhighlight %}

To begin with, the `Company` class knows explicitly about the `Person` class therefore coupling itself to that class.
Also in this scenario the `Company` class knows about a message that is not destined to itself (`get_personal_information`) but to the `Person` class. Again this creates similar coupling to what you would get in the first scenario, as now if you were to change the method `get_personal_information` in some way that would alter it's behaviour or even a simple name change, that piece of code in the `Company` class  would break.<br/>


- if an object knows the arguments that a message requires
- if an object knows the order of the required arguments

{% highlight ruby %}
 class Company
   ...
   def set_employee_record
     details = Person.new('Joe', 26).get_personal_information
     # some code
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
Here the `Company` class knows that a `Person` object *needs* two arguments, *name* and *age*. It also knows that they have to come in a certain order. The *name* argument must be passed first before the number, otherwise it changes everything, aka breaks.

##### Solutions to the above

According to Sandi few ways of dealing with the above are the following:

- Dependency Injection
- Isolating External Messages
- Using hashes for initialisation arguments

In the first scenario **Dependency Injection** could be used to eliminate from the `Company` class, the direct reference to the `Person` class:

{% highlight ruby %}
class Company
  attr_reader :person
  
  def initialize(person)
    @person = person
  end
  
  def set_employee_record
    person.get_personal_information #=> an object that responds to the method 
    # some code                     #=> get_personal_information, will do
  end 
  ...
end

Company.new(Person.new)
{% endhighlight %}

By doing the above two things are achieved at once; First the creation of an instance, in this case `Person`, is isolated in one place only. If we needed to use that object again, we don't need to instatiate it again. It can be reused anywhere and if a change is needed, we would only need to change it in one place, at the initialize method.
A second benefit is the fact that now the dependency on `Person` is _exposed_; meaning it's at a very visible location (`initialize`) and by being so, it acts as a friendly warning to other people that might end up using the `Company` class.

If we were to combine the first scenario with the second, we would have a slightly more suitable example were the other two solutions come to play:
{% highlight ruby %}
class Company
  attr_reader :person
  
  def initialize(person)
    @person = person
  end
      ...
  def set_employee_record
    details = person.get_personal_information
    # some code
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

Isolating external messages here is specifically applicable to the `set_employee_record` method. By having a foreign method inside of it, it relies on the fact that, that method will remain stable for a good amount of time, meaning it won't change any time soon. If the result of the `get_personal_information` was used in conjuction with other data to generate necessary information for the `Company` object, then the ability to easily change that piece of code becomes very important. The situation can get worse by having other methods relying on that same method; duplication would ensue, amongst many other things.

A solution to this problem would be to isolate that one line into a method that you can pass around wherever is needed. By doing so, you give yourself again the ability to change an important part of the code, at one specific point only:
{% highlight ruby %}
class Company
  attr_reader :person
  
  def initialize(person)
    @person = person
  end
      ...
  def set_employee_record
    details = get_personal_details #=> only cares about what, not how
    # some code
  end

  def get_personal_details
    person.get_personal_information #=> if change is needed, only this changes
  end
  ...
end
{% endhighlight %}
When it comes to the arguments list, a solution for avoiding issues like passing arguments in the wrong order and errors by missing some is using a hashwith the arguments and defaults:
{% highlight ruby %}
class Person
  attr_reader :name, :age
  
  def initialize(args)
    @name = args[:name] || 'Joe'
    @age = args[:age] || 27
  end
end
{% endhighlight %}

What you achieve with this is that that piece of code now doesn't rely on the order but rather the keys. Of course that in itself is a dependency too but a slightly better one in this case. Having the default's also helps if no arguments are passed and in a scenario like that you will be in for a treat once your program gets filled with Joes :)

I tried keeping the post short but certain things needed each other. I need to keep reminding myself that the above are simply guidelines to help design better software. They will not be needed always. That's the mistake I commit frequently; trying to optimise the design too early, goes against the three principles of TDD which I will leave for another post.

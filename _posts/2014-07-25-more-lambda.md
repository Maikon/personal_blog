---
layout: post
title: "More Lambda"
category: post
---

In yesterday's post I talked about how I used lambdas to make the game more flexible and how a few interesting things came up. Before that I need to correct a mistake from yesterday where I said that the `until` loop was removed as a result. It was instead transferred into an appropriate lambda.

So some interesting things about lambdas:

- it can be called in a couple of ways

{% highlight ruby %}
say_hi = lambda { puts 'Hi' }
say_hi.call
#=> Hi

say_hi = lambda { |name| puts "Hi #{name}" }
say_hi.("Joe")
#=> Hi Joe
{% endhighlight %}
<br/>
- it's scope agnostic (doesn't know the environment it is created in) if you store it in a constant

{% highlight ruby %}
class Person
  attr_reader :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  CALCULATE_AGE = -> { age + 10 }

  def age_in_ten_years
    CALCULATE_AGE.call
  end
end

person = Person.new("Joe", 20)
person.age_in_ten_years
#=> NameError: undefined local variable or method `age' for Person:Class
{% endhighlight %}

Similarly when you use the @ symbol in front of age you get the following:


    NoMethodError: undefined method `+' for nil:NilClass

However, if you wrap it inside a method things change:

{% highlight ruby %}
class Person
  attr_reader :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  def age_in_ten_years
    -> { age + 10 }
  end
end

person = Person.new("Joe", 20)
person.age_in_ten_years.call
#=> 30
{% endhighlight %}


Another way is to pass in the instance variable as an argument:

{% highlight ruby %}
class Person
  attr_reader :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  CALCULATE_AGE = ->(age) { age + 10 }

  def age_in_ten_years
    CALCULATE_AGE.call(age)
  end
end

person = Person.new("Joe", 20)
person.age_in_ten_years
#=> 30
{% endhighlight %}

I've certainly learned some really interesting things when it comes to lambdas and there's still a few question marks in my head as to why certain things act in such a way but the experience has provided me with plenty of food for thought. I'll make sure I report any further findings on the matter.

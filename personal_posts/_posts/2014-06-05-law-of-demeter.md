---
layout: post
title: "The Law of Demeter."
category: post
---
### The Law of Demeter

The Law of Demeter states that a method of an object should invoke only the methods of the following objects:

- **itself**
{% highlight ruby %}
class Car
  ...
  def wheel_size
    10
  end
  ...
end

Car.new.wheel_size # Demeter is happy with this
#=> 10
{% endhighlight %}
<br/> 

- **its parameters**

{% highlight ruby %}
class Car
...     
  def total_wheels_price(wheel)
    wheel.price * 4 # Demeter is happy with this too
  end
end

Class Wheel
  ...
  def size
    10
  end
  ...
end

car = Car.new 
car.total_wheels_price(Wheel.new)
#=> 40
{% endhighlight %}
<br/>    

- **any object it creates/instantiates**

{% highlight ruby %}
class Car
...
def wheel_perimeter
  wheel = Wheel.new
  3.14 * wheel.size # Happy here too
end
{% endhighlight %}  
<br/>    

- **its direct components**

{% highlight ruby %}
class Car
  attr_reader :wheel
  
  def initialize(wheel)
    @wheel = wheel
  end
  
  def wheel_size
    wheel.size # Demeter is happy with this too
  end
end

Class Wheel
  ...
  def size
    10
  end
  ...
end

car = Car.new(Wheel.new) 
car.wheel_size
#=> 10
{% endhighlight %}
The advantages of following the law are significant. The classes that result from following the rule tend to be more adaptable and thus easier to change as well as to move around since they have less dependencies. To demonstrate what I mean by that, I'll give you an example that violates the law:
<br/>

{% highlight ruby %}
class Garage
  attr_reader :cars
  
  def initialize(cars)
    @cars = cars
  end
  
  def total_price_of_cars
    total = 0
    cars.each do |car|
      ...
      # does something
      ...
      car.wheels.each do |wheel|
        total += wheel.price  # Demeter would not be happy with this
      end
      # more code
    end
  end
end
{% endhighlight %}
     

There's few things wrong with this method but for this post we just need to focus on the LOD. Here the Garage takes an array of cars upon initialisation. It then has a method which after looping through the array, it calculates the total price of them. The problem here lies deep in the iteration. After getting each car it access its wheels method which would return the wheels and then it reaches even deeper by accessing the wheel's price method.

One of the easy ways to discover whether the law is violated, is the amount of dots in method calls. The above might be a bit disguised but it could also be written like this: `cars.first.wheels.price`.
This method has too much knowledge and it reaches too deep to get what it wants.
`Garage --> Car --> Wheel --> price`.
If anything changes within the `Wheel` class and especially within the price method, `total_price_of_cars` would fall part.

One way of solving this would be to use *delegation*:
{% highlight ruby %}
class Car
  ...
  def total_wheels_price
    wheel.price * 4 # create method inside Car class that access its wheel
  end              # component. Point 4 above
end

class Garage
  ...
  # inside the loop eliminate and replace appropriate lines
  def total_price_of_cars
    total = 0
    cars.each do |car|
      # some code 
        total += car.total_wheels_price  # Demeter happy mode back on
      # more code
    end
  end
end
{% endhighlight %}

Now the Garage only knows that Car has a method it can call to get the price of each wheel. It doesn't know of any details about the implementation. It simply knows it will get the price.

As I'm still at a very early stage in my career, I'm doing my best to put into words and examples a lot of the concepts that I need to understand in order to become a better developer. If you think that my understanding of the principle discussed or the examples I'm using are wrong somewhere, please point it out. It's the only way I can improve and learn.

I found the following links to be immensely helpful when doing my research:

- [The Law of Demeter (Thoughtbot)](https://www.youtube.com/watch?v=RwO7SdSP_E8&list=PL8tzorAO7s0jx03fMEzVzsq-zNHOWHvSZ&index=18)
- [Wikipedia page](https://en.wikipedia.org/wiki/Law_of_Demeter)
- [Misunderstanding the Law of Demeter](http://www.dan-manges.com/blog/37)

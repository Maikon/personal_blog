---
layout: post
title: "Connascence"
category: post
---

###Connascence

- The birth of two or more things at the same time.
- The act of growing together

Or in software terms:

***A relationship between two or more elements of software in which changing one necessitates changing the others in order to maintain overall correctness.***

The above is something which the late Jim Weirich gave a [talk](http://www.confreaks.com/videos/93-aac2009-the-grand-unified-theory) on which I thought it was quite interesting. During the talk Jim explained how there's many theories about what constitutes good design (following SOLID principles, Law of Demeter, DRY etc) but felt that a more unifying theory was missing.

He points to [this](http://www.amazon.com/Every-Programmer-Should-Object-Oriented-Design/dp/0932633315) book as the source of the word *connascence* and how it applies to software development.

In essence I think it's an alternative way, or at least similar, of looking at the concept of Coupling. You will see why from the examples taken from the talk how these two relate. During the talk, Jim points to four specific cases of Connascence:

- Connascence of **Name**
- Connascence of **Position**
- Connascence of **Meaning**
- Connascence of **Algorithm**<br/>
<br/>

###### Connascence of Name

Example:
{% highlight ruby %}
class Customer
  ...
  def email
    # does something
  end
end

def send_mail(customer)
  customer.email
  ...
end
{% endhighlight %}

In the above example there is connascence of name. What this means is that the `send_mail` method is *attached* to the `email`method in the Customer class since we're calling it explicitly within it. Therefore the above two share connascence based upon the name `email`. So if we were change the name of the `email` method to something different, the `send_mail` method must change too. Another part of the above code where connascence exists is the parameter `customer` passed in to the `send_mail` method. If that parameter changes, the line inside the method referring to that parameter (`customer.email`), must change too. This is a much more *soft* connascence but one nonetheless.

###### Locality

Jim also points out that locality also matters. For example connascence within a class will be stronger than the connascence between two different classes. Or at least that's how it should be since the internal pieces of a class will have to work with each other to serve the purpose of that class whereas the connascence between two classes should be weaker as to avoid strong coupling. What comes to my mind when thinking about this concept and how it can be achieved, is the idea of [polymorphism](http://maikon.github.io/2014/06/03/polymorphism.html) and [duck typing](https://en.wikipedia.org/wiki/Duck_typing).

The general rule of thumb according to Jim: 

***As the distance between software elements increases, use weaker forms of connascence.***

###### Connascence of Position


Example:
{% highlight ruby %}
def process_orders(list_of_pairs)
  list_of_pairs.each do |order, expedite|
    # handle an order
  end
end

# the list of pairs are in this form
[
  [Order.find(3), true],
  [Order.find(5), false]
]
{% endhighlight %}  

In the above, the order within each pair in the list is important. The `process_orders` method expects a list where in each pair, the order will come first and then the expedite flag saying whether its true or false. In this scenario there is *connascence of position* . The degree of this connascence here is low since there's only two items. If there's more however then there's a higher degree and should be avoided.

The ideas found in the talk are certainly interesting and are a good way of analysing the relationship between different parts. I definitely started looking at my own code through the lens of connascence and I find the result worthy of more experimentation.  

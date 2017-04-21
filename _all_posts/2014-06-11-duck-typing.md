---
layout: post
title: "Duck Typing"
category: post
---

From wikipedia:

***When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck.[[1]](https://en.wikipedia.org/wiki/Duck_typing#cite_note-1)***

Duck typing is a term that for a long time was not clear to me. I was introduced to the term a while back but I think because of a lack of context, I failed to really understand what it meant.

One my tasks for last week was to create a method which would take any shape and return its type and perimeter. The exercise would serve as good lesson as to what duck typing is. It's worth noting that by that point I had understood duck typing quite well so the exercise was quite handy in practising what I had learned.

As always I started off with a test and built it up but I will skip to where it shows the value of duck typing. I will be posting other material where I will show an exact process but for now here's the test in question:
{% highlight ruby %}
it 'handles two shapes' do
  s1 = double(type: :square, perimeter: 10)
  s2 = double(type: :rectangle, perimeter: 15)
  expect(PerimeterCalculator.new.perimeters_for(s1, s2)).to eq({square: 10,
                                                                rectangle: 15)
end
{% endhighlight %}

In the test there are two doubles with two methods each that share the same names. This is a good hint as to what duck typing will enable. s1 will represent a square and its `type` method returns `:square` when called. Its `perimeter` method returns 10. The same happens with s2 which simply represents a rectangle. The `perimeters_for` will take s1 and s2 as arguments and return a hash with the name of the shape and its perimeter. So I'm passing some data and I'm getting a new data structure back.

Another test that is relevant is the below:
{% highlight ruby %}
it 'handles anything that is not a shape' do
  expect(PerimeterCalculator.new.perimeters_for(1)).to eq({})
  expect(PerimeterCalculator.new.perimeters_for('shape')).to eq({})
  expect(PerimeterCalculator.new.perimeters_for(:shape)).to eq({})
 end
{% endhighlight %}
    
This one checks to see whether the method can deal with objects that are not Shapes. What constitutes a shape? Well the first above gives an indication. Anything that responds to the `type` and `perimeter` methods, is a shape. If it quacks like a duck, it is a duck.

The code that makes the above pass is this:
{% highlight ruby %}
# duck typing to the rescue
def perimeters_for(*shapes)
  shapes_hash = {}
  return shapes_hash if !shapes.first.respond_to?(:perimeter) # here
  shapes.each do |shape|
    shapes_hash.merge!(:"#{shape.type}" => shape.perimeter) # and here
  end
  shapes_hash
end
{% endhighlight %}  
  
Using the splat operator `*` I can take any amount of arguments as an array, so If I'm only passing in 1 item, I will get an array with a single item. The line starting with `return` checks to see whether the first item responds to the method `perimeter` and if not, it returns the empty hash. I could also write this last line as "***ask the the first object that comes through if it quacks. If it does, then it's a duck so let it through.***". This is where duck typing comes handy. The operations inside the method don't care about the incoming objects as long as they respond to the two methods they care about.

An example of shapes that would qualify for the method above is the below:
{% highlight ruby %}
class Rectangle # acts like a duck
  ...
  def type
    :rectangle
  end
  
  def perimeter
    (length + width) * 2
  end
end

class Square # acts like a duck
  ...
  def type
    :square
  end
  
  def perimeter
    side_size * 4
  end
end
{% endhighlight %}

A better implementation would be instead of returning an empty hash when the object is not a duck type, raise an error indicating that the specific object does not meet the requirements for that method.

There's many more situations where duck typing becomes useful and really shines but for now this is an example I feel I can explain best. I certainly look forward implementing various forms of it and report back with some more real world examples.

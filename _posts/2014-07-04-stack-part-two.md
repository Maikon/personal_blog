---
layout: post
title: "Building a simple Stack in Java. (Part 2)"
category: blog
tag:
- data structures
- java
---

In the post yesterday we started putting together a simple Stack using Java and we stopped at the last failing test which was this:

{% highlight java %}
  @Test
  public void multipleItemsCanBePoppedFromTheStack() {
    stack.push("woot");
    stack.push("wat");
    assertEquals("wat", stack.pop());
    assertEquals("woot", stack.pop());
  }
{% endhighlight %}

To make the test pass and not break any previous ones we'll put the right implementation in place and will use a [Linked List](https://en.wikipedia.org/wiki/Linked_list) for that. I'll post the full code and explain each part individually. Before that I put together a small diagram which can be found [here](http://cl.ly/image/0p0U0N3r1N2f), that explains the browser example I gave in yesterdays post in a more visual way.

The final code implementing a Linked List and making the final test pass follows:
{% highlight java %}
public class Stack {
  private int size;
  private Link head;

  private class Link {
    private String value;
    private Link next;

    public Link(String item){
      value = item;
    }

    public String getValue() {
      return value;
    }

    public Link getNext() {
      return next;
    }

    public void setNext(Link item) {
      next = item;
    }
  }

  public boolean isEmpty() {
    return size == 0;
  }

  public int getSize() {
    return size;
  }

  public void push(String item) {
    Link newItem = new Link(item);
    Link oldHead = head;
    head = newItem;
    head.setNext(oldHead);
    size++;
  }

  public String pop() {
    Link oldHead = head;
    head = oldHead.getNext();
    return oldHead.getValue();
  }
}
{% endhighlight %}

There's a few things going on. First of a new class was introduced `Link` which has two attributes; A `value` which will be a string and `next` which will be an `Item`. When a new `Link` is created it takes a string as an argument which sets the value of the `value` attribute. We then create two getters `getValue` and `getNext` plus a setter `setNext`, for the `next` attribute.

Next up is the part which took me a while to wrap my head around, possibly because I was visually imagining it from the wrong perspective. Lets start with the `push` method. When a new item is pushed in, it does the following:

- Creates a new `Item` and stores it into the local variable `newItem`. The string that comes through sets the `value` of that `Item`
- Assigns the `head` attribute to the local variable `oldHead` which will be of type `Link`
- Set the value of `head` to `newItem`
- Then set the `head`'s `next` value to the `oldHead` value

If you are confused don't worry, I'm pretty sure most people including me are when they look at the above, at least the first couple of times. To help I've put together another [diagram](http://cl.ly/image/1q3B2W0k1e22) continuing with the browser example which hopefully make things more clear. The only part from the above that is there from before is the increment of `size`.

The `pop` method is a bit more straightforward:

- Create `oldHead` local variable with the value of `head`
- Set the value of `head` to `oldHead`'s `next` value
- Return the value of `oldHead`

Again [this diagram](http://cl.ly/image/2c1r1B300D3f) might be helpful in order to understand the chain of events. I also found looking at both events, `push` and `pop`, together helps even more so [this](http://cl.ly/image/190l091I0r2v) is a shot of both diagrams combined. The one thing that initially confused me was thinking about a Linked List as an array. That's the first thing I was thinking we would need to implement at some point and that thought didn't leave me even after we had done the final solution. Once I went through it a couple more times and drew some diagrams it became more clear.

Implementing the above was an interesting challenge but it helped me understand a commonly used data structure which in turn increased my knowledge of the fundamentals of programming significantly. I would definitely encourage anyone to do the same in any language they're learning. Practising these sort of things is essential and will be doing more of these as I move forward.

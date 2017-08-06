---
layout: post
title: "Building a simple Stack in Java. (Part 1)"
category: blog
tag:
- data structures
- java
---

Starting yesterday and continuing today me and Mike worked on some fun exercises with Java and one of them included building a Stack. The exercises can be found [here](http://introcs.cs.princeton.edu/java/43stack/).

The definition of a Stack is as follows:

***A stack is a basic data structure that can be logically thought as linear structure represented by a real physical stack or pile, a structure where insertion and deletion of items takes place at one end called top of the stack.***

A basic implementation of a stack is also called a LIFO. This means that the (L)ast item that goes (I)n, is the (F)irst one that comes (O)ut. A good example of this is explained in the link above. When you are using a browser and you click a link, the link gets added (push) to the stack. When you want to revisit the previous link and you click the back button, it pops the link from the stack. Last In, First Out. Therefore the main two operations of a Stack are *push* and *pop*.

In this post I'll show how we started by implementing the following three methods:

- **isEmpty()**
- **push(String item)**
- **pop()**

The first test looked like this:

{% highlight java %}
  public class StackTest() {
    @Test
    public void stackIsEmpty() {
      Stack stack = new Stack();
      assertTrue(stack.isEmpty?())
    }
  }
{% endhighlight %}

To make it green:

{% highlight java %}
  public class Stack() {
    public boolean isEmpty() {
      return true;
    }
  }
{% endhighlight %}

This is probably the most "degenerate" case as Uncle Bob would call it. The implementation is very simple and definitely not what we really want but it will make the test pass and for now that's all that matters.

The next test is the following:

{% highlight java %}
  @Test
  public void anItemCanBeAddedToTheStack() {
    Stack stack = new Stack();
    stack.push("1")
    assertFalse(stack.isEmpty?())
  }
{% endhighlight %}

For this one to pass and not break the first test, I introduced a `size` attribute. Initially I put a conditional but Mike corrected me by saying if the conditional can be avoided then it should as it adds complexity since with a conditional in this situation would create a two way stream. A good blog post that relates to this point can be found [here](http://blog.8thlight.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html) by Uncle Bob. The code for the second test to pass then looks like this:

{% highlight java %}

  public class Stack() {
    private int size;

    public boolean isEmpty() {
      return size == 0;
    }

    public void push(String item) {
      size++;
    }
  }
{% endhighlight %}

Next up was the test checking that a new stack starts with a size of zero. I can tell what you're thinking and you're right; this will pass out of the box although the reason for this test is to simply create a method that returns the size without touching the attribute itself:

{% highlight java %}
  // Additional refactoring to remove duplication
  public class StackTest() {
    private Stack stack;

    @Before
    public void setUp() {
      stack = new Stack();
    }
    ....

    @Test
    public void sizeIsZeroForANewStack() {
      assertEquals(0, stack.getSize());
    }
  }
{% endhighlight %}

Code to get this green:

{% highlight java %}
  public int getSize() {
    return size;
  }
{% endhighlight %}

Then we check if we can pop an item from the stack:

{% highlight java %}
  @Test
  public void anItemCanBePoppedFromTheStack() {
    stack.push("1");
    assertEquals("1", stack.pop());
  }
{% endhighlight %}

Again the least amount of code that could get this to pass:

{% highlight java %}
  public int pop() {
    return "1";
  }
{% endhighlight %}

Now you might think I'm taking too many small steps and you are right. I'm deliberately taking these steps however since writing the least amount of code has significant advantages. There's a saying in the programming community that says ***The best code is no code at all***. This can lead to surprising results. It's amazing how little code you can write and get things to work by following this principle and we did have a moment like that with Mike while doing the above. Another benefit is the simplicity that comes out of following simple choices vs adding complexity that might not even be necessary.

The next test was the one that we thought would force us to actually put some more smart code and it looked like this:

{% highlight java %}
  @Test
  public void multipleItemsCanBePoppedFromTheStack() {
    stack.push("1");
    stack.push("2");
    assertEquals("2", stack.pop());
    assertEquals("1", stack.pop());
  }
{% endhighlight %}

Of course with us being lazy and following the least code principle this was the resulting code to make the above test looked like this:

{% highlight java %}
  public int pop() {
    return Integer.toString(size--);
  }
{% endhighlight %}

Of course a small change was necessary as the above almost feels like cheating since we would be passing longer strings and not just string numerals:

{% highlight java %}
  @Test
  public void multipleItemsCanBePoppedFromTheStack() {
    stack.push("woot");
    stack.push("wat");
    assertEquals("wat", stack.pop());
    assertEquals("woot", stack.pop());
  }
{% endhighlight %}

And of course now the test will brake. In the next part I will show how the actual implementation looked like and explain it a bit more as it took me a while to wrap my head around. I'll explain how the stack works a bit more especially with [Linked Lists](https://en.wikipedia.org/wiki/Linked_list) using some diagrams as well. Hopefully there's plenty food for thought here until tomorrow.

---
layout: post
title: "Enter Java"
category: blog
tag:
- java
- tdd
---

One of my tasks for this week's iteration is to learn and practise the Roman Numerals Kata but in Java. It's always exciting, for me at least, whenever I get the chance to learn a new language and even more so when practising a familiar kata. In show a small sample in this post with more to come once I understand certain things better.

First thing to note is with Java you more or less have to use an [IDE](https://en.wikipedia.org/wiki/Integrated_Development_Environment) and the most commonly used one is [Intellij](https://www.jetbrains.com/idea/). I got some help from Felipe to get the project going on since his background is in Java. The first thing that struck me was the amount of steps I had to go through just so the right environment was in place with the right file structure etc. Understandably the steps will be less after the initial set up but even then it's still not as easy as simply creating two files and get going as it is in Ruby or maybe it is but I just don't know it yet.

The first test I wrote looked like this:
{% highlight java %}
  @Test
  public void testConvertZero() {
    RomanNumeral converter = new RomanNumeral();
    String empty = "";
    String result = converter.convert(0);
    assertEquals(empty, result));
  }
{% endhighlight %}

Ruby version for comparison:
{% highlight ruby %}
  it 'converts zero' do
    expect(RomanNumeral.convert(0)).to eq ''
  end
{% endhighlight %}

After playing around and trying to understand what exactly is going on, I figured out that I could have it in one line like so:
{% highlight java %}
  @Test
  public void testConvertZero() {
    assertEquals("", RomanNumeral.convert(0));
  }
{% endhighlight %}

Minimum implementation to get the above to pass:
{% highlight java %}
  public class RomanNumeral() {
    public static String convert(int number){
      return "";
    }
  }
{% endhighlight %}
<br/>
#### Code breakdown
From wikipedia [page](https://en.wikipedia.org/wiki/Java_(programming_language)):

***Public***

> The keyword **public** denotes that a method can be called from code in other classes, or that a class may be used by classes outside the class hierarchy. The class hierarchy is related to the name of the directory in which the .java file is located.

***static***

> The keyword static in front of a method indicates a static method, which is associated only with the class and not with any specific instance of that class. Only static methods can be invoked without a reference to an object. Static methods cannot access any class members that are not also static.


***void***

> The keyword void indicates that the main method does not return any value to the caller. If a Java program is to exit with an error code, it must call System.exit() explicitly.

The way I understand it: ***static*** more or less denotes a class method, ***void*** denotes that I don't expect the test method to return anything, ***public*** is used to make a class accessible across the system.

I managed to complete the Kata with tests guiding the development and I have to admit the final algorithm was generated more "organically" because of the nature of Java. Even though the solution is pretty much identical, there's definitely more things to consider than in Ruby where pretty much anything goes.

I'll repeat the kata a few more times and expand on the results in more detail as well as with a screencast in due time but it's fair to say that I'm liking it more than I thought I would.

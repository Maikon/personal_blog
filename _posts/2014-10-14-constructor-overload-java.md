---
layout: post
title: "Constructor Overloading in Java"
category: blog
---
Starting this week, I'm getting hands on with Java. I had a previous encounters during my apprenticeship with couple of katas but that was simply testing the waters. Having to build TTT from scratch is more like a deep dive in the language.

There's many interesting things about Java and certainly coming from a Ruby background, many "limitations" but an interesting one is the idea of ***Overloading*** and more specifically ***Constructor Overloading***.

What Constructor Overloading boils down to is this:

***A class may have more than one constructor.***

The *constructor* is what initialises the object and if your background is also in Ruby then the most familiar one will be the `initialize` method. Unlike Java though, in Ruby you can only have one `initialize` method and this where *Overloading* comes in. *Overloading* essentially means having a constructor with the same name but with different method signatures. A method signature is essentially the list of parameters in a method. 

This is one of the things I like so far about Java as it allows you to build objects in a variety of ways. Now you could add a level of customisation in Ruby by adding defaults but it's certainly not as flexible and clean. Having separate constructors shows your intents more clearly and keeps things more organised.

I'll use some of the code I wrote today to demonstrate this:

```java
public class Board {
  private final List<String> grid;
  
  public Board() {
    this(Collections.<String>nCopies(9, null));
  }
  
  public Board(List<String> grid) {
    this.grid = grid;
  }
}
```

In the above I have two constructors with the same name `Board` but with different signatures. One takes no arguments while the other takes a list of strings. The interesting part here is the `this` keyword. What `this` does in the first constructor is that it calls the second one with a default value. You can also write the first constructor like so:

```java
public Board() {
    new Board(Collections.<String>nCopies(9, null));
  }
```
but there is no need really since `this` is rather clear. A good post on `this` can be found [here](http://stackoverflow.com/questions/577575/using-the-keyword-this-in-java).

There were two main motives behind the above. One was to aide with immutability; instead of making changes to an existing board, I simply return a new one with the move that was made. The other one which is certainly more important is to help with testing. Having the two constructors I can in my tests do the following:

```java
@Test
  public void itReturnsFalseIfBoardDoesNotHaveWinner() {
    Board board = new Board();
    assertEquals(false, board.hasWinner());
  }

  @Test
  public void itReturnsTrueIfBoardHasWinner() {
    Board board = new Board(asList("X", "X", "X", "", "", "", "", "", ""));
    assertEquals(true, board.hasWinner());
  }
```

The convenience can be seen in the second test. I can initialise a `Board` with a pre-specified grid which makes my tests much easier to setup.

It's worth noting that when overloading the constructor, the numbers of parameters and their type matters. If I tried adding another one with same number of arguments(1) but of the same type:

```java
  public Board(List<Integer> grid) {
    this.grid = grid;
    }
  // this will not even compile since it takes the same type of argument (List)
```

Being new to Java I made several mistakes while trying to get to this direction through testing but I did have some help from a colleague much more experienced in Java than me who helped me remove some duplication and showed me some neat tricks.

I'm pretty sure I barely scratched the surface with Overloading and I can imagine myself using it more down the line for which I will be writing more about.

Some helpful resources on *Overloading* in general:

- [Wikibooks](https://en.wikibooks.org/wiki/Java_Programming/Overloading_Methods_and_Constructors)
- [SO-Overloading Best Practises](http://stackoverflow.com/questions/1182153/constructor-overloading-in-java-best-practice)

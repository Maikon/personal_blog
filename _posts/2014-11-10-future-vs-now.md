---
layout: post
title: "Future Flexibility Vs Current Knowledge"
category: post
---

One of the most important principles I've learned so far during my apprenticeship is that when developing software more often than not it is vital to defer important decisions until more knowledge is available. 

Trying to reach an abstraction too early can lead to the wrong abstraction and as Sandi Metz once [said](http://www.databasically.com/2014/06/23/all-the-little-things/) ***"Duplication is far cheaper than the wrong abstraction"***. Trying to adapt a piece of code to accommodate extra clients that only exist in your head, will put you on a path filled with pain most of the time. 

What happens though when you think you have enough evidence to support the construction of that abstraction? What happens when you are pretty sure of what the future holds?

A scenario that occurred to me recently is the motivation behind this post. As I was working on my TTT implementation in Java, every decision I made was based on the assumption that the board would have a 3x3 grid. If you've played TTT before it's safe to say that's a valid and safe assumption. However a common exercise during the apprenticeship is to extend the functionality of the board so that it accommodates a 4x4 grid. 

I knew that this was likely to happen however I kept the code as it was. One of the reasons I did so was because at the time a 3x3 grid was my only requirement. Following Kent Beck's [rules](https://en.wikipedia.org/wiki/Test-driven_development ) of least amount of code for a test to pass combined with refactoring, meant that I had an implementation of a 3x3 that was very clear. The code for example that would give me the rows of the grid looked like this:

```java
public List<Line> getRows() {
    return asList(getLine(0, 1, 2),
                  getLine(3, 4, 5),
                  getLine(6, 7, 8));
  }
...
private Line getLine(int first, int second, int third) {
   List<String> positions = asList(grid.get(first), 
                                   grid.get(second),
                                   grid.get(third));
   return new Line(positions);
  }
```
The values for a 3x3 are hard-coded which makes it rather easy to understand what is going on. On the other hand though flexibility is lost. I was ok with that though because striving to write code which is easy to understand is a personal priority.  

Eventually I had to change my board so that it would accommodate the 4x4 grid and there were few changes I had to make, mostly where the hard coded values were but to see the difference here's what the equivalent code for the above looked like:

```java
public List<Line> getRows() {
  List<Line> rows = new ArrayList<>();
  for (int i = 0; i < size; i++) {
    rows.add(getLineRange(i * size, size - 1));
  }
  return rows;
}
...
private Line getLineRange(int startIndex, int count) {
  List<String> positions = new ArrayList<>();
  int endIndex = startIndex + count;
  for (int position = startIndex; position <= endIndex; position++) {
    positions.add(grid.get(position));
  }
  return new Line(positions);
}
```
It's worth noting that the above required changes in the `Line` class as well. The code can now accommodate a different sized board and it's certainly more flexible. However it also requires more mental energy points than the original one. Can it be made more readable? Possibly. Would it need to abstracted more than the above? Maybe. 

You could argue that this piece of code could have been implemented from the beginning at the "refactoring" part but to me at least that's not what the refactoring part is about. It is easy to fall in the trap of "refactoring" after a green test to the point where you're adding new functionality that is not covered by the requirements or the tests that are currently in place. Refactoring is cleaning up existing code ***without changing its external behaviour***. Improving the maintainability and extensibility does not equal with extending it **now** if the need is not present.

Like I mentioned above this scenario might not be the best example since I had a good idea that this would happen. Nevertheless I chose to follow this path as an exercise for future endeavours. I've been guilty many times in the past of always trying to look too far ahead in my efforts of making my programs easy to change. What I always ended up with was a nightmare comprised of unnecessary tests, unnecessary abstractions and maintenance costs that would slow me down for days. This is not to say that I don't do this anymore but trying to maintain discipline with a temptation such as this, will hopefully pay off in the long run.

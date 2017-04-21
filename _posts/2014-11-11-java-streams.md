---
layout: post
title: "Working with the Java Stream API"
category: post
---

Having been doing most of my work in the last few weeks in Java, one of the things I missed the most from Ruby was the [Enumerable](http://www.ruby-doc.org/core-2.1.4/Enumerable.html) module. This was especially the case when working with TTT.

One of the areas where I'd find it quite useful was when checking if a winner existed on the board. In Ruby what I had was something like this:

```ruby
def winner?
  winning_lines.any? { |line| line.all? { |cell| cell == line.first } }
end
```

The `winning_lines` method would return a two-dimensional array with all the possible lines that lead to a win (rows, columns, diagonals). Since each line was represented as just another array, I would call `all?` on it to check whether all the elements are the same by comparing each one to the first one of that line. The `any?` does what you might have guessed; it checks if any one line from all possible ones has a winner.

The above fits nicely in one line and is quite expressive. Ruby at its best. In Java to achieve a somewhat similar result I ended up creating a `Line` class and within that I had methods that would do that type of check on each line. For example on my initial version which I only had a 3x3 board, part of the code inside the `Line` class looked like this:

```java
public class Line {
  private String first;
  private String second;
  private String third;
  
  public Line(List<String> positions) {
    first = positions.get(0);
    second = positions.get(1);
    third = positions.get(2);
  }
  
  private boolean positionsHaveSameMark() {
    return first.equals(second) && second.equals(third);
  }
}
```
Extending the `Board` class to accommodate a 4x4 board the above could be modified to cater for it like so:

```java
public class Line {
  private List<String> positions;
  
  public Line(List<String> positions) {
    this.positions = positions;
  }
  
  private boolean positionsAreSame() {
    List<Boolean> qualifiedPositions = new ArrayList<>();
    for (String position : positions) {
      if (position.equals(positions.get(0))) {
        qualifiedPositions.add(true); 
      }
    }
    return qualifiedPositions.size() == positions.size();
  }
}
```
It's hard to argue as to which one is more concise and readable when comparing the above with the equivalent Ruby example. The winner is clear.

### Enter Java Streams

Since Java 8 the [Stream API](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html) was introduced and knowing a colleague who had used it and was happy with it, I decided to investigate further. After doing a little bit of a research and tinkering, I ended up with this solution for getting the same result as the previous example:

```java
private boolean positionsHaveSameMark() {
  return positions.stream().allMatch(position -> position.equals(positions.get(0)));
}
```
It took me few minutes to understand the syntax but that was probably because of my Ruby background and lack of the functional one. What happens here is not that different from the Ruby example at the top. It's really the part starting with `all?`. We get the positions and turn them into a stream which the way I understand it seems like an iterator. Then the `allMatch()` method is called with a [predicate](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html) (condition) and each element is checked against that condition. The `position -> position.equals(positions.get(0))` part is the equivalent of Ruby's `|x| x == line.first`. I actually think Java's syntax in this case is clearer than Ruby's.

On a similar note, I modified the method that was checking if the positions on a line were not empty since they were initialised as empty strings. The code for that is now this:

```java
private boolean positionsAreNotEmpty() {
  return positions.stream()
                  .filter(position -> !position.isEmpty())
                  .findAny()
                  .isPresent();
}
```
Here I filter the positions checking each one if it's empty. If the condition is satisfied it will add that element to an [Optional](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) object. I then call `findAny()` which simply checks the `Optional` object if it has any positions that matched the original condition. That in turn returns another `Optional` object on which I call the `isPresent()` method to verify whether the result from `findAny()` came back empty or not.

Again coming from Ruby, on a first glance I expected that `findAny()` would return a boolean rather an `Optional` but having just started with streams, I shall look further into why an empty `Optional` would ever be needed.

So far working the Stream API has been a nice experience and looking at the API it seems that for code like the above to be possible, a lot of heavy lifting has been taken care of. I'm definitely looking forward to refactoring some of my code to make use of it. You can find out more details on what I used [here](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html).

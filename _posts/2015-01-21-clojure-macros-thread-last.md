---
layout: post
title: "Clojure Macros: ->> (Thread-Last)"
category: blog
---

One of the most popular features and advantages of Clojure or any
Lisp-based language are
[macros](http://en.wikipedia.org/wiki/Macro_%28computer_science%29#Syntactic_macros).
Today I had the chance to use one as a part of a refactoring I was doing
so I got a first-taste of a Clojure [macro](http://clojure.org/macros), in
this case the `->>` macro commonly-known as ***thread-last***.

**Definition:** *Threads the expr through the forms. Inserts x as the
last item in the first form, making a list of it if it is not a
list already. If there are more forms, inserts the first form as the
last item in second form, etc.*

If you're like me and find definitions without any code examples
incomplete, then worry not:

```clojure
(defn winner? [board]
  (some true? (map #(all-equal-not-empty %) (lines board))))
```

This is the code I was refactoring today. Starting from the right and innermost call:

- Get all lines of the board
- map each line through the `all-equal-not-empty` function
- check the resulting list to see if any line returned `true` based on
  the predicate

The above was refactored to the following:

```clojure
(defn winner? [board]
  (->> (lines board)
       (map (partial all-equal-not-empty))
       (some true?)))
```
You can think of the above as *"Take the result of `(lines board)` and
feed it as a last argument to the next form. Then repeat the process."*
So the next form in the code is `(map (partial all-equal-not-empty))` so
in this case `map` takes two arguments, the function which a collection
will be mapped through and a collection. `(lines board)` is a collection
and will step in as the second argument. The result of that will then go
through the `(some true?)` in similar fashion.

Another part of the refactoring was `partial` which I will explain in
another post. There's a time and place for everything and I think the
use of the `->>` macro in this situation does make the code slightly more
readable and clear.

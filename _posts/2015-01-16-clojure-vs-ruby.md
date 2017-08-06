---
layout: post
title: "Clojure vs Ruby (so far)"
category: blog
---

When talking with people about Clojure the words you'll hear more often
used to describe the language are ***simple***, ***consise***,
***transparent*** and ***elegant***. Having little to no experience with
Clojure I couldn't really understand what those words meant in the given
context. I'm still no Clojure expert but I'll try and explain what those
words mean to me now and having worked with Clojure exclusively for the last
couple of weeks.

<br/>
### One Syntax To Rule Them All

Clojure is simple. Simple to me also means consistent. Consistency makes
things easy. Being a Lisp dialect, Clojure inherits the simplicity of it's
syntax which is built upon [S-expressions](https://en.wikipedia.org/wiki/S-expression):

```clojure
;; Function on the left, arguments on the right
(+ 1 2)
=> 3

;; Filter collection based on predicate
(filter number? ["a" 1 "b" 2])
=> (1 2)

;; map the collection through the anonymous function
(map #(clojure.string/capitalize %) ["john" "joe" "wayne" "wade"])
=>("John" "Joe" "Wayne" "Wade")
```
<br/>

### (= Concise Elegant)

*"Expressing much in few words; clear and succinct":*


```clojure
;; Filter with this predicate, this collection
(filter number? ["a" 1 "b" 2])
=> (1 2)
```
I come from a Ruby background and I still think the above is very
elegant. In Ruby some of the ways to achieve the above would be:

```ruby
["a" 1 "b" 2].select { |item| item.is_a?(Fixnum) }
#=> [1, 2]

["a" 1 "b" 2].grep(Fixnum)
#=> [1, 2]
```

Now there is the difference in that you are calling methods on objects
in Ruby vs a [pure function](https://en.wikipedia.org/wiki/Pure_function)
but I think once the basic premise of each paradigm is explained, the
Clojure syntax is, in my opinion, easier to read and understand. I also
think that a functional language such as Clojure might possibly be a
better option for a beginner to start with than an OO one especially on a
conceptual level but that's a post for another time.

<br/>

### Conclusion

So far I'm enjoying Clojure a lot more than I thought I would. What I
also find impressive about it is that I end up writing less code for
the same programs that I wrote in Ruby. Though not as rich as the Ruby
core library, Clojure's one provides just enough functions which once
combined can yield powerful results. I'll be describing some of
Clojure's features as I go in the next posts.

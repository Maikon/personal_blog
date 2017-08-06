---
layout: post
title: "Enter Clojure"
category: blog
tag:
- clojure
- functional
---

Some of the stories that I've been working on this past week, involved an API which was written in [Clojure](http://clojure.org/). My only previous encounter with the language came in the form of [koans](https://github.com/functional-koans/clojure-koans) so it was quite challenging looking at actual code that was designed to do certain things.

Last night I attended my first [Coding Dojo](http://otfrom.wordpress.com/2010/10/26/faq-how-much-do-i-need-to-know-before-i-come-to-the-dojo/) which was organised by the London Clojurian community. The group I ended up with was focusing on the exercises on [4Clojure](http://www.4clojure.com/problems) as a good introduction to the language.

Having done some of the koans, certain things looked familiar yet it was still interesting to see how different the mindset has to be when writing Clojure vs writing it in a language like Ruby. For example if you wanted to iterate through a collection and do something to each element, in Ruby you would do it like so:

{% highlight ruby %}
[1, 2 , 3, 4, 5].map |number|
  number * 10
end
#=> [10, 20, 30, 40, 50]
{% endhighlight %}

You get the list of items from the left, then call a method called `map` on it which will then construct a new array with the result of executing the block on each element, in this case multiplying each one by 10.  In Clojure you would do the following:

{% highlight clojure %}
(map #(* % 10) '(1 2 3 4 5))
;; '(10 20 30 40 50)
{% endhighlight %}

Here, we call the `map` function and we pass two arguments to it. The first argument is a custom function `#(* % 10)` which takes an argument, in this case indicated by the `%` placeholder. The second argument is the collection which we will iterate through, in this case the list `'(1 2 3 4 5)`. The way it reads, each element of the list from the right, goes through the custom function and comes out the other side inside a new list. My thinking and explanation is very likely lacking clarity due to my limited experience with Clojure, but you can see how different it is from a conventional OO language like Ruby.

Another feature in Clojure that I find fascinating is multi-variadic functions:

{% highlight clojure %}
(defn multi-variadic
 ([] "Without arguments!")
 ([one two] (str "The result of adding "one " and " two " is: " (+ one two)))
 ([one two three] (str "Adding three arguments is: " (+ one (+ two three))))
)

(multi-variadic)
;; "Without arguments!"

(multi-variadic 1 2)
;; "The result of adding 1 and 2 is: 3"

(multi-variadic 1 2 3)
;; "Adding three arguments is: 6"
{% endhighlight %}

In the above we create a function called `multi-variadic` which has 3 different [signatures](https://en.wikipedia.org/wiki/Signature_(computer_science)).  When the function is called with no arguments, it will use the first signature and simply return the string `"Without arguments!"`. When given two arguments, it will use the second signature and execute the relevant function which in turn has two functions inside of it (*stay with me*). One for adding the two arguments `(+ one two)` and another one which constructs the string `(str "The result of adding "one " and " two " is: " (+ one two))`. The `str` function in this case essentially takes 6 arguments; 3 strings, 2 numbers and a function which then evaluates to a number:

- "The result of adding "
- one
- " and "
- two
- " is: "
- (+ one two)


`str` is another interesting function but i'll leave that for another time.  The fact that you can have one function that will yield different results depending on the arguments given to it, is quite impressive at least for me and my small knowledge of languages.  I could probably think of way of doing something similar in Ruby but it would probably involve see some kind of if statement and possibly a decent amount of duplication in the body of the function but Clojure deals with it quite elegantly.

Even though I'm fairly new to Clojure and the syntax being unusual, I have to admit that I kind of like it. There's something "clear" about seeing and thinking in terms of pure functions that produce the same result every time you call them. I definitely look forward learning more as I go.

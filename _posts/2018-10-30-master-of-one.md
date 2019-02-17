---
layout: post
title: "Jack of all trades, Master of One"
authors: ["makis-otman"]
tag:
- Coding
- Debugging
- Apprenticeship
- Craftsmanship
category: blog
---

___This blog post originally appeared on the [8th Light Blog](https://8thlight.com/blog/makis-otman/2018/10/30/master-of-one.html).___

> Jack of all trades, master of none

The above is a well-known saying that people tend to use when referring to
polymaths or generalists. It's often said in jest, though the underlying
assumption is a bit more bleak. This is especially true if you notice its usage in [other
languages](https://en.wikipedia.org/wiki/Jack_of_all_trades,_master_of_none#In_other_languages):

> Many talents is no talent
>
> -*Japanese version*

Rather ominous right?

If you're a generalist, do not despair. In this blog post I will make the
case that the nature of generalists is a blessing in disguise, and that in
reality they are true masters of one specific and very important skill:
problem solving.

## Mastering Problem Solving

A few months ago I attended a workshop by Dan North on delivering quality
software faster. During the workshop, Dan asked the following question: _"Is
code a liability or an asset?"_. He asked us where we stood on the matter,
quite literally, on a scale of _"all code is a liability"_ to _"all code is an
asset"_. In the end he said _"All code is liability"_. Inevitably, someone
posed the question _"What about my job?"_ to which Dan responded: _"Your job is not to write code. Your job is to solve problems. You are a problem solver."_

As consultants at 8th Light, we encounter this scenario very frequently.
Switching projects often means having to learn new tools and new domains.
The ability to dissect a problem and quickly get unstuck—regardless of the technology—is
[crucial](https://twitter.com/ctford/status/1016425570513678336). We are
generalists by nature.

With this in mind, what are some techniques that generalists possess and
employ that makes them great problem solvers?

## Narrowing Scope

> The smaller the problem the easier it is to tackle

When dealing with the unknown, few things if any can magnify the issue as
much as _scope_. The more information you're trying to keep
together in your head, the bigger the challenge. Size can be your
friend or your worst enemy.

### Divide and Conquer

This first step in this process is to dissect a bigger problem into smaller
problems which can be focussed on individually. Let's look at the following example:

_"Build a command line app in Go that takes command line arguments and prints them
back to the console."_

Let's also assume you've never worked with Go or at least have done very little.
Here are some of the problems we are faced with:

1. Learning Go
2. Building an executable in Go
3. Printing to the console in Go
4. Passing command line arguments to a Go executable
5. Parsing command line arguments in Go

Initially this might have seemed like a big task, but breaking it down helps
you see the smaller pieces to which you can focus on one thing at a time.
You can literally google each of one those lines and they will give you results
that more or less give you the answers you're looking for.

### Learning a new tool

The first thing in our list is **"Learning Go"**. This on face-value seems like a
very big and broad challenge. Where do you start? What should you
master first? How many things do you need to learn?

These are all valid questions but they won't help you. The question that
will help you the most is the following:

> What can I safely _**ignore**_ for _**now**_?

This is very important. It's simply another mechanism for reducing scope.
You don't need to master every aspect of Go to build an executable or
print to the console. You need to learn just _enough_ to complete the task at hand.

## Effective searching

Knowing how to search for things is an art in itself. When searching you
want to make it easy for the search engine to find things for you whilst
also making the process efficient for you. Compare the following two search
strings:

> show me how to build a Go executable

**VS**

> build go executable

Both will produce more or less the same results though one requires fewer
keystrokes. The amount of keystrokes you save is not really the main goal
here. The goal is to train yourself in breaking a problem down to its
essence. This not only helps you when searching for things but also when
you are building something since it can help you identify sub-problems.

## Debugging

When dealing with the unknown you are almost certainly going to face
errors. Some may be easy, others not so easy. They can also be very
demoralising. I faced, and face many errors and challenges every day. What
has helped me persevere over the years is the following belief:

> Every error is the result of some action *somewhere* and is *probably* solvable

As part of this belief, I developed an intuition when it comes to searching
for information that can help me solve the problem. The below diagram is an
attempt to make this intuition more concrete:

![debugging](/assets/searching.png)

If you internalise the above, then any problem is solvable. When you believe
that and follow a similar approach, then the real question is whether the
thing you are trying to solve is worth your time.

## Short Feedback loops

A key element of the flow described above are short feedback loops.
Whether you're trying to print a string on the console, debug an error, or
anything else, you want to make sure that you have a short feedback loop in
place. What does that mean? It means receiving feedback from your actions
as quickly as possible.

> Minimise the distance between your action (input) and the outcome (output)

A good example of a short feedback loop mechanism is a
[repl](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop).
You type some input, press enter, and you get immediate output (feedback)
back. In our Go example, the reason why the second step in the process is
building the executable is because that will give you a very fast feedback
loop when trying to fill in the rest. You start with printing a `hello
world`, and then you move toward parsing arguments. For both of these you
have an extremely fast feedback loop with `./run-my-executable`.

Another mechanism is TDD and unit tests. If you don't know where to start, traditional print statements can
also provide you with extremely fast feedback—though you need to be extra meticulous in the sense that you have to place them strategically in your code. Add too many and you get information overload. Too little and you're not getting enough information. Even the order in which you add or remove them makes a difference.

Let's take an example. You are faced with the following
problem: your code is returning an error, you don't have tests and you're
not sure where to start. The first step is to establish which code path is
exercised, i.e _"Ah, it's class A, method-B which calls method-C from class D and
its own private method-E."_. Then you proceed with narrowing down the scope:

![debugging](/assets/debugging.png)

## Resetting

If all of the above fails, sometimes the best course of action is to throw
something away and start fresh. It's much easier to keep going down the rabbit
hole once you're in it than it is to take a step back. If you're using Git,
you can exit the rabbit hole with a `git reset`. This will give you the
time to reset mentally and approach the problem with a fresh perspective.

## Reframing polymathy

I hope this blog post left you with some techniques to improve your own
problem-solving ability. I equally hope that if you're a generalist, I've
given you a different perspective from which to view yourself and others;
one which makes you feel proud. We can also change the original quote
ever so slightly so that it makes people more curious and allows you to
expose your greatest skill:

> Jack of All Trades, Master of One.

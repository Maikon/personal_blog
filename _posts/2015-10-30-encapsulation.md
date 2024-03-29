---
layout: post
title: Encapsulation
category: blog
tag:
- programming
- languages
- software design
---
During the week I took part in Sarah's IPM, an apprentice I'm co-mentoring, and at some point we were discussing the idea of encapsulation and how it applies in Java and in general.

Encapsulation is the idea of packaging data and operations on that data together. To a certain extent it also promotes information hiding and does not allow for some parts of an object to be changed from outside. In the case of a Tic-Tac-Toe Board for example you would have a method `mark_position(1, 'X')` which would tell the `Board` object how to alter its underlying data structure. You would structure it in a way that only that method would cause the underlying data structure to change and nothing else.

While we were having the discussion on the above idea, I started rethinking why encapsulation is such an important idea, at least in the Object-Oriented realm. One of the main reasons as I mentioned above, is so that changes(mutation) on state can be controlled. In OO this is crucial. If you're relying on a specific value for your code to work and then someone else changes it, you're in trouble. If your sharing the same object what happens when you want to run things in parallel? Controlling change or more specifically _mutation of state_ becomes tricky.

In this sense can encapsulation be considered a form of [defensive programming](https://en.wikipedia.org/wiki/Defensive_programming)?

With the re-emergence of functional programming, the idea of encapsulation is in question. Perhaps the most basic idea of this paradigm is that we have data and functions that apply transformations to that data. State does not exist, at least in the traditional OO notion. Every time you apply a function to some data, you get a new value back. Immutability therefore guarantees that side-effects that would occur in a language that allows mutation, are no longer an issue. Is encapsulation as an idea necessary in functional programming? Perhaps not.

It's still an idea that I'm exploring as I'm learning more about functional programming and I will revisit this in the future.

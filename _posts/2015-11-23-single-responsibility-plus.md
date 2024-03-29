---
layout: post
title: Single Responsibility +
category: blog
tag:
- software design
- software principles
- coupling
---
One of the most commonly known principles in software design is the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle). It is usually used as a guideline for creating new software as well as analysing and improving existing one.

SRP however can be applied to other things as well. Git commits are an example. A good commit has a well defined purpose expressed through the message. A good message indicates very clearly what was done. If the message goes along the lines of _"Add x and y"_ then perhaps the commit is _adding too much_ and it's better to split it into two.

A good commit is also atomic in its nature. One should be able to checkout/revert to a commit and run all the specs without any issues. It should stand on it's own in this sense. If a commit fails the specs, it is very likely that it depends on another commit that contains the fix. If so, we have a [temporal coupling](https://blog.ploeh.dk/2011/05/24/DesignSmellTemporalCoupling/) between two (or more) commits which in this case can be eliminated by joining those commits into one.

This is just another area that I found this principle to be applicable and the more I think about the software development process, the more I realize it applies to many more.

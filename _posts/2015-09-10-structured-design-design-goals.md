---
layout: post
title: Structured Design (Design Goals)
category: blog
tag:
- software design
- computer science
- books
- architecture
---

Yesterday's [post](https://makisotman.com/structured-design-intro) introduced the idea of Structured Design. In this post, I will explain what I understand as the objectives of Structured Designed. Its main goal is good design and to achieve that goal, Structured Design focuses on the following characteristics of a system:

1. Efficiency
2. Reliability
3. Maintainability
4. Flexibility
5. Generality
6. Utility

*Efficiency* looks at how well resource management is performed in a system as well as how it can be improved. A system that manages its limited resources well is considered efficient.

*Reliability* focuses on how often a system which is considered repairable, will fail and the time between each failure. This is often described by the acronym **MTBF** (mean-time-between-failure). The third characteristic, *Maintainability* is to a certain extent related to *Reliability*. This characteristic describes the amount of time it takes to restore the system from an error. It is often referred to as **MTTR** (mean-time-to-repairs). This includes the time it takes to discover, analyse and fix the error. If the amount of time is relatively small, then the system is considered maintainable. *Systems Availability* is described by the following formula:

`MTBF / (MTBF + MTTR)`

*Flexibility* describes the ease in which a system can be extended and altered so that it can perform various tasks without changing the core theme of the system. *Generality* describes how generic a system can be. This goes in tandem with *Flexibility* since what is meant by generality is not how big a system should be and how many needs it should cover. What Generality means is that a system should be build from many smaller components that are easy to compose and decompose. If done correctly this means that you should be able to combine these components in many various ways and by doing so you achieve *Generality*.

*Utility* is the ease in which a system is available in the outside world and how easy it is to interact from a user's perspective.

This concludes a brief description of the goals that Structured Design tries to achieve. I expect these objectives to be explored further as I progress further in the book.

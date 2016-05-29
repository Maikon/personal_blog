---
layout: post
title: "Keep asking Why."
category: post
---

### Keep asking Why.

The main reason for starting this blog was for me to be "forced" to put into words whatever it is that I'm learning. Many times in the past (still happens now), I'll read or watch something, understand it at that point, but when the time comes to explain it to someone else, I'm staring at a blank board in my mind. 

Whenever I'm stuck with a specific problem or in general interested in learning about a principle from which I could gain an immediate benefit, I tend to do a decent amount of research. When I do find the solution to a problem that's been bothering me for a while, I tend to make it work and quite often move on to the next thing. Here lies the problem.

##### The five Whys

From Wikipedia:

**_"The 5 Whys is an iterative question-asking technique used to explore the cause-and-effect relationships underlying a particular problem. The primary goal of the technique is to determine the root cause of a defect or problem."_**

I must admit that I don't use it as much as I would like to or should do, but the times that I used it, it always benefited me immensely. I remember some days during the MA course, I would start the day by writing down two questions:

- **_What must I achieve today for the day to be a success?_**
- **_If I encounter problems, how will I deal with them?_**

On the first question I would list the objectives of the day, usually 2-3 specific things. On the second one and after some thinking I came up with an answer that is related to the five Whys technique. Whenever faced with a problem that would cause major pain, I told myself that the solution would be to turn into a *root seeker*. For some reason at the time that I came up with this term, I thought about heat seeking missiles that are used in warfare and like a missile that looks for a heating generating source and chases it till it reaches it, I would treat that problem in the same way. Chase it all the way down to its root and really understand the source of the problem.

Example from Wikipedia:

*The vehicle will not start. (the problem)*
  
- Why? - The battery is dead. (**first why**)  
- Why? - The alternator is not functioning. (**second why**)
- Why? - The alternator belt has broken. (**third why**)
- Why? - The alternator belt was well beyond its useful service life and not replaced. (**fourth why**)
- Why? - The vehicle was not maintained according to the recommended service schedule. (**fifth why, a root cause**)

Very often when I'm dealing with a programming related problem, quite often the answer or at least the most valuable answer, never lies at level 1:

An example that I was recently doing:

*I need to initialise the Display object with two separate streams, one for input, one for output. (the problem)*

- **Why?** - Because I want to use those two to record incoming as well as outgoing messages.
- **Why?** - Because it will make it easier to test if I am getting the right result.
- **Why?** - Because I can use the StringIO class to read and write from.
- **Why?** - Because I don't want to stub something which I don't own with a specific implementation.
- **Why?** - Because if my tests are coupled to the implementation details of a method and not the result, they will be harder to change.

Now, my analysis is probably still flawed somewhere but I'm at a better level of understanding now than when I was before doing it. Now at least I have a platform from which I can continue exploring and learning.

Doing the above allows me to understand why I am doing what I'm doing and instead of randomly trying things to make something work, I make conscious decisions based on a advantages vs disadvantages analysis. I'm not saying trying things out is bad, I do it a lot myself and have discovered many nice things as a result of it but in order to learn something in a really deep level the 5 whys or a similar approach is the way to do it.

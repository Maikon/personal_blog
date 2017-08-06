---
layout: post
title: "Simplicity == Complexity"
category: blog
---

Simplicity is a very good thing, I'm definitely a fan. In all walks of life you will find the best things tend to be the simplest ones at the same time. People strive towards designing and keeping things as simple as possible and this also extends to software. It was one of the first things that we were taught at MA and one the things I keep reading everywhere; People always prefer a solution that is easier and simpler to understand than a fancy and complicated solution.

When it comes to programming, simplicity is often associated with keeping things small and clear. Especially in the Object-Oriented world, breaking long classes full of procedural code into smaller and well defined pieces, is generally considered good practise as well as good design. However, simplicity comes at a cost too. 

Sandi Metz said in her excellent talk, [Rules](https://www.youtube.com/watch?v=npOGOmkxuio), gave a good hint at what that cost may be. She said that once you abandon the practise of writing long procedural code and instead opt for writing smaller objects, you will never understand the system in the same manner that you did when you writing long procedural code. The part where she explains that can be seen [here](http://youtu.be/npOGOmkxuio?t=15m0s).

A quote which I also came across which I found quite fitting is the below: 

***"UNIX is basically a simple operating system, but you have to be a freaking genius to understand the simplicity"***

Simplicity is often achieved through many layers of complexity. I see Sandi's point every day in the code that myself and the rest of the team write. 

First objective when you have a failing test according to Kent Beck, is to make it green at all costs. Be it heavy duplication or long complex code, it has to go green. Once that is done, well, you [extract till you drop](https://sites.google.com/site/unclebobconsultingllc/one-thing-extract-till-you-drop).  Sometimes that means extracting 3-4 extra methods out of that one method in order to separate concerns but also for things to be more clear.  Suddenly though you have 4 methods that interact with each other at some point and what seemed as a simple solution in your head, now requires extra thought and focus. You have to track each step from the first method call onwards.

Like all things when developing software, refactoring can be pushed to the extreme; a point where it actually does more damage than good. For example Ruby's [reduce](http://www.ruby-doc.org/core-1.9.3/Enumerable.html#method-i-reduce) method from the Enumerable module was probably put there for a good reason. You can do some very impressive and rather tricky things with it that would usually require much more lines of code. Is it easier to understand than the normal longer piece of code that would normally be needed? Not always. Do you have to create 3-4 methods to replace the call to *reduce*? Perhaps not. Where do you draw the line when enforcing the [command-query separation principle](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation)?

At the end of the day everything is a trade-off. The above are questions that I expect to face on a daily basis. What's interesting is how the perspective changes based on the situation. It's all quite tricky most of the time but nevertheless part of the journey...

---
layout: post
title: Out Of The Tar Pit (Control)
category: blog
tag:
- software design
- papers
---
One of the things I'm currently reading is ["Out of the Tar Pit"](https://github.com/papers-we-love/papers-we-love/blob/master/design/out-of-the-tar-pit.pdf). It's a well known paper that many people reference and it happens to be the first paper that we started reading in our newly formed book/paper club at the office.

From what I understand so far, its main goal is to 1) identify and point out the reasons of the complexity that is found in software and 2) solutions to those problems.

One of the things that adds complexity according to the paper, is flow of control and the obsession developers have with it. It is a problem even on a language level. Consider the following example from the paper:

```ruby
a := b + 3
c := d + 2
e := f * 4
```

Here it seems that's there's an order in which things are executed. Mentally, that's the first thing a programmer looking at this code will think. This is a common pattern that exists in many languages. Yet there is no important order here. What the goal was, and should be, is to specify the _what_ of the operations but instead we must specify the _how_ by having to write the code above in a way that it forms an artificial ordering.

This is a problem in the sense that the language above is adding a mental barrier towards understanding that piece of code. More importantly understanding the _what_. If I wanted to split the above code into smaller components then my initial thought might be that the ordering must be preserved even though it does not matter. I have to do extra mental work to deduct that there's no ordering in this case.

This simply adds to the complexity of software, especially when we're dealing with something more complex than the above, which is the majority of the time.

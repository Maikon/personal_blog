---
title: Navigating & Debugging unknown Ruby (and Rails) code
layout: post
tag:
- ruby
- rails
- debugging
- tools
category: blog
---
Navigating code is hard. Even code that you have written. Navigating code
that other people have written? Now that's **extra** hard.

Despite all the best practices we have at our disposal and our eagerness to
write the best possible code that a fellow human can understand, the end
result will always be seen as a challenge to work with. Why? Well, there
are a couple of reasons. They key ones are _context_ and _domain
knowledge_.

Context is the combination of business requirements, constraints, team dynamics etc that led to
the code be designed and written that way. Domain knowledge is the
knowledge that can help someone understand the context they're in much
faster. For example, I currently work at a [Rail~~s~~
company](https://loco2.com/). A good understanding of how the Rail industry
works is not a must have necessarily but it can make my job significantly easier.

Very handy:
```
a.methods.grep(/arrival/)
```

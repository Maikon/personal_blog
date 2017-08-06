---
layout: post
title: "Back on Track"
category: blog
---

After a two-week break from writing, it feels good to be back here. I took the break as I felt I was spending too more time than I needed on writing a blog post, time which could definitely be used on actual coding. I learn so much by writing these posts though that I had to get back to it asap.

For my last week's iteration I had to complete my GUI for tictactoe game, meaning the four different game types (human vs human, human vs computer, computer vs human, computer vs computer) had to be playable. It took me a while wrapping my head around Qt and how it deals with events but eventually I got all the above to work which was nice. A user now can choose what type of game they want to play (or watch). Doing the last task revealed the basic nature of my minimax implementation as it was so slow for the first move. As a result, I added, with much help from Christoph, a raw version of [Alpha-Beta pruning](https://en.wikipedia.org/wiki/Alpha-beta_pruning) which made things much faster.

I intend on writing a longer post or a small series on various Qt related things and the minimax algorithm but since I still feel a bit "rusty" I'll just share some resources that taught me some cool things the last couple of weeks:

- [OOP: You're Doing It Completely Wrong](http://vimeo.com/91672848)
- [The Ruby Object Model](https://www.youtube.com/watch?v=X2sgQ38UDVY)
- [How I Explained REST to My Wife](http://www.looah.com/source/view/2284)
- [Tell Above, and Ask Below - Hybridising OO and Functional Design](http://michaelfeathers.typepad.com/michael_feathers_blog/2012/03/tell-above-and-ask-below-hybridizing-oo-and-functional-design.html)

I also managed to give my first public talk last week and that went much better than I thought. At an event prior to that met with current students at Makers Academy which resulted in me having 3 mentees which I will be helping out during the remainder of their course so I'm looking forward to the experience.

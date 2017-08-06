---
layout: post
title: "Value of Constraints"
category: blog
tag:
- coderetreat
---

This week we had the pleasure of meeting another craftsman from the Chicago Office, [Mike Ebert](http://www.8thlight.com/team/mike-ebert) who happened to be in town. Yesterday we had an interesting "exercise" with him called "Code Retreat". The idea originates from something the famous Ruby developer Corey Haines [started](http://coderetreat.org/group/organizers-hosts) which essentially is an exercise where a problem is given, usually [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life), to pairs and 45 min are allocated per session at the end of which you have to throw away the code you've written, change partner and start again. Now I didn't even mention the best part of this exercise. For each session you are given a certain constraint with which you have to operate with. There's a list [here](http://coderetreat.org/facilitating/activity-catalog) to give you an idea but the ones we were given were:

- No naked primitives unless in the constructor or within a method
- Maximum of four lines per method
- Immutability across the board, no change of state or side effects
- No talking between the pair

I think if I asked you to pick what you think would be the most interesting one, you would agree the fourth one on the list would be the winner. However I learned an incredible amount by each one of them.

The first three are more of a technical nature and each poses an interesting challenge, especially the first and third on the list. The one I want to focus on however is the one we agreed was the winner; no talking. Mike said to us when he introduced the constraint that this where usually the cleanest code gets written. It's something that you could show to your mum and she would be able to get an idea of what's happening even though she never encountered a single line of code. It was "mum friendly".

I paired up with Felipe and we started working. Naturally our methods were more verbose. I generally do like my methods to be more verbose than most people I've seen as I find well named code expresses its intentions clearly without the need for comments. Also it helps me remember why I did each thing when I look back after months.
Another side effect was that more thought went into each test so that whoever was the navigator, could understand exactly where the driver was heading.

There was a funny moment where Felipe wrote a small piece doing some calculation which seem slightly complex. I wrote the following on a piece of paper **"!Mum"**, which translates to not Mum friendly. Felipe got the cue and promptly changed it.

At the end of all the sessions we all agreed that it was one of the best experiences we had. Being able to see how people approach the same problem in so many different ways was a real eye opener. Every time I thought I had a good idea of what was going on, I would see the code someone else wrote and re-evaluate. Good communication is also essential. Listening to your partner and understanding her/his intent is crucial in order to be able to provide the necessary feedback and suggestion to the problem.

In general I think setting constraints is extremely powerful technique that helps when trying to come up with alternative solutions. It leads you to explore paths that you wouldn't otherwise consider and those paths might be substantially better than the one you were planning on taking. It reminded me of [this](http://www.ted.com/talks/phil_hansen_embrace_the_shake) superb TED talk on limitations. It is something I'll definitely try and apply on my day to day tasks.

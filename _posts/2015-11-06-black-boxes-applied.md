---
layout: post
title: Black Boxes Applied
category: blog
tag:
- software design
- computer science
- books
- architecture
---
In a [recent post](https://makisotman.com/structured-design-black-boxes) I wrote about the concept of __"Black Boxes"__ that I learned through _"Structured Design"_. Yesterday we got to applied it with my student apprentice and the effect it had on us was quite remarkable.

We started building a small program that would take input from the user and search a file which would be parsed for a result. We started talking about what would be a good point to start with our tests but we were both not sure where to begin. At that point I pointed to the white board. With a pen in hand I started asking my apprentice as to what functionality we're aware of. "We need to parse the data" he said. I drew a box that had a label __ParseData__. On the top of the box, I drew an arrow pointing down with the label __"raw\_data"__ and underneath the box an arrow pointing downwards with the label __"formatted\_data"__. This represented a black box with a known input (raw_data) and a known output (formatted\_data). The goal of that module from a high level view was clear:

![some](/assets/parse_data.svg)

We repeated the same thing for other pieces we felt they were necessary (ConsoleIO, QueryData) following the same steps as above. By the end of it, the pieces of the solution were well defined.

The biggest takeaway from the experience however was the fact that we decided upon a different starting point than what we initially planned on. Our thought was to perhaps start with the parsing of the data. However after drawing the boxes and discussing their different properties, it became clear that the starting point should be the __QueryData__ for two reasons. For one, it was actually simpler to tackle than dealing with anything that involved parsing. The second point though which was the most important was the fact that by starting on the __QueryData__ we could describe in our terms and in our tests _how we wanted the parser to behave_. The last point helped us identify very clearly what the __ParseData__ module should give us and thus we focused on getting that desired output.

This is a powerful technique which can help break down all kinds of problems regardless of size and complexity. Applying it consistently is the tricky part...

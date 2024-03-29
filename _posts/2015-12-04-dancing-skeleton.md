---
layout: post
title: Dancing Skeleton
category: blog
tag:
- practices
- workflow
---
Yesterday I [wrote](https://makisotman.com/constraints) about one of the patterns of effective delivery as described by Dan North in his [talk](https://vimeo.com/43659070). Another pattern from there which I found interesting, is a pattern called ___"Dancing Skeleton"___.

The idea and the name is based on Alistair Cockburn's concept of the ["Walking Skeleton"](http://wiki.c2.com/?WalkingSkeleton). The idea here is that when you're creating a system you try to implement the smallest amount of functionality that will provide you an end-to-end function.

___"Dancing Skeleton"___ is an extension of the above in the sense that once you have that end-to-end functionality in place you continue a few steps further. If you're building a web app for example you would ask yourself: "How fast can I get a 'Hello World' <rails/phoenix/etc> app deployed to <heroku/EC2/etc> with monitoring, logging, and everything else required for it?". The goal is to explore the path to production and see what it takes to have the full stack that you care about in place.

A traditional approach is to write the application and the logic and then at some point deploy it and add things as you go. By following the ___"Dancing Skeleton"___ approach your primary goal is to make deploying to production and managing that process as fast and smooth as possible. I recently tried a tiny example using this approach by trying to deploy a Phoenix "Hello World" app to Heroku just to see how fast it can be done and it was quite enlightening.

Some of the patterns Dan mentions in his talk are quite interesting and this is certainly one of the most interesting ones. It can be used effectively to not only make the deployment process easier but also as a way to learn more about deployment in general especially if your experience in that area is lacking.

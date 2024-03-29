---
layout: post
title: Keep It Small
category: blog
tag:
- software design
- practices
- workflow
---
Yesterday I was discussing with other fellow crafters what constitutes a good pull request. I started the conversation by saying how much I disliked big PRs and the reasons why they don't fall under the "best practices" section.

When we write code we follow principles such as the Single Responsibility to help us improve our code and design in general. One of the reasons SRP is great is because if done correctly you should be able to open up a class and understand very quickly what its purpose is. It would be much harder though if that class was doing a bunch of things.

The same applies to a PR. If a PR is relatively large then it's probably introducing a lot of new ideas. Ideas that could be broken into separate, more digestible PRs. From the reviewer's perspective, 2-3 small PRs is always better than a single large one. Small, focused PRs that encapsulate an idea well, are much easier to go through.

Reading the code of someone else is already hard enough and a skill in and of itself. When you send a big PR, you're also sending the message that you don't care about the reviewer. A more mindful approach is to find a good point where you can break off and create a smaller PR for one part of the solution. By doing so you're far more likely to get that PR reviewed thoroughly and merged quicker to allow you to move on with the rest.

Keep it simple. Keep it small. Be disciplined.

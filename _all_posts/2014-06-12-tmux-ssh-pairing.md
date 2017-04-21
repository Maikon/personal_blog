---
layout: post
title: "Pairing using Tmux and SSH"
category: post
---

[Tmux](http://tmux.sourceforge.net/) is a new tool I've started using since joining 8th Light as it's an extremely efficient way of boosting your workflow and manage your various projects. Tmux stands for terminal multiplexer. What does this mean? 
Tmux page: 

*It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal. And do a lot more.*

The main benefit I get from this so far as there's so many shortcuts and workflows one can create, is the fact that I can create a new session which one way to think about it is a terminal within a terminal. You create a session do some work on a project, detach from it and the session with the setup will still run on the background even if you restart the terminal at which point you can re-attach to it and continue from where you left off.

These are some resources for Tmux that should be helpful:

- [Tmux crash course](http://robots.thoughtbot.com/a-tmux-crash-course)
- [Tmux cheatsheet](https://gist.github.com/henrik/1967800)
- [Tmux: A simple start](http://www.sitepoint.com/tmux-a-simple-start/)

Tmux really shines when it comes to pairing though. Yesterday I paired with Uku and we did it over a secure [ssh](http://www.openssh.com/) connection and using a pretty sweet gem called [github-auth](https://github.com/chrishunt/github-auth). We set it up in such a way that Uku could login on my machine and see exactly what I was seeing on my screen and do work on it as if he was using my computer. One specific benefit of this is the fact that Uku could use his own vim configuration and general setup so this removes the barrier of getting used to someone's else setup when pairing.

###### Setup

To install tmux and assuming you have [Homebrew](http://brew.sh/) installed, all you need to do is head over to the terminal and run `brew install tmux`.

Next open up ***System Preferences*** and go to the file called ***Sharing***. There on the left side you will see a panel with options and you need to enable the ***remote login*** option.

Back in the terminal, install the gem mentioned above with `gem install github-auth`. You can see a list of options on how to use it on the github project but what I did was run the following command:

`gh-auth add --users=heruku --command="$( which tmux ) attach -t pair"`

The above raised an error when I run it because I was missing a `authorized_keys` file so if the same happens when you try to run it you will get the instruction to create one: `touch /Users/makis/.ssh/authorized_keys`.

What the command starting with gh-auth did was go up to Uku's profile on github (heruku) and get his public ssh keys and add them to my `authorized_keys` file. The next part simply says that Uku can only connect to my machine if I have a tmux session running called `pair`. If not Uku won't be able to connect regardless if I have other sessions going on. Even if Uku logs in a session I create named pair, he will lose connection if I kill that session.


Out of the different ways I've tried pairing with people I possibly enjoyed the above the most in the sense that I wasn't leaving my environment yet I could still communicate and pair effectively. Clearly the driver-navigator notion is not as clear as when you have only one keyboard between two people but still this a great substitute especially when pairing remotely.

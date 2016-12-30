---
layout: post
title: "Git Disciplined"
category: general
---

_This post appeared originally on [8th Light's Blog](https://8thlight.com/blog/makis-otman/2015/07/08/git-disciplined.html)._

In our day to day work, one of the most common tools we as developers
use is [Git](http://en.wikipedia.org/wiki/Git_%28software%29). In a
nutshell, Git is a tool that makes managing and working with multiple files easy. It helps us keep track of the files and monitor any changes that happen to them.

It's a great tool that enables efficient collaboration between
individuals and teams. But, as with everything, a great amount of
discipline is required for everything to run smoothly, particularly
between teams. I'm going to share some of the common actions I take to
remain disciplined with Git and explain how those actions also help my colleagues.

## Keep The Focus

One of the most common pieces of advice you will hear is that you
should ***commit early and often***. This is something that I learned
the hard way, but committing often and in small chunks is
helpful for a variety of reasons.

For one, you provide yourself small
breadcrumbs along the way that you and your team can follow if things go
wrong.

Let's say I'm working on a large feature that requires quite a bit of
work, and the algorithm is comprised of small pieces that I can test
drive one by one. As soon as I have one of those parts working, I will
commit it using **WIP** (work in progress) as the first word of the
commit message. For example:

{% highlight bash %}
  WIP Create feature part 1
{% endhighlight bash %}

I will use this "flag" for one of two reasons: either I feel
there's some serious refactoring to be made, or this is a small part
of a bigger concept.

The former reason relates to one of the best pieces of advice I read
from Kent Beck about test-driving code: ***make it work, make it right,
make it fast***.

It doesn't matter how you got it to work, what's important is committing
it first and applying all the refactoring you need later. Having that
safely under Git's cloak, I can continue working without worrying about
that specific piece of code. The `WIP` flag is there to indicate that
the code is a work in progress and not the end result. I will explain how to remove that flag when you're done, too.

This can be a hard habit to learn though, and I still often find myself
getting carried away while coding. Having finished the feature, I keep going with another feature plus some refactoring. I have quite a few files that
need to be committed, but they relate to different concepts. If I'm lucky
enough, I can just split and add the files directly that relate to a
specific concept, but that's hardly ever the case. Luckily, Git provides
another feature that comes handy in this situation: Git add with the
[-p](http://git-scm.com/docs/git-add) (p for patch) flag:

{% highlight bash %}
  git add -p file
{% endhighlight bash %}

The above will put me into an interactive mode where I can choose which parts of the file I want to add to the staging area. This gives me the opportunity to bundle up associated pieces of code from different files.

Although not used as frequently, it's also possible to remove parts of the file that are already in the staging area with:

{% highlight bash %}
  git reset -p file
{% endhighlight bash %}

While small and early commits are useful, they don't always
make for good reading. Sending a pull request with 10+ commits that
includes things like *"Use snake_case"* and *"Cleaning up whitespace"* is
not very helpful for the person/people reviewing it. You also don't want
to have commits starting with **WIP** since they are no longer part of
something which is in progress. This brings us to the last step
of this process: [git rebase](http://git-scm.com/docs/git-rebase).

With `git rebase`, you can rewrite history in a way that allows you to
make your commits more focused. All the ones that relate to a specific
feature and start with **WIP** should be squashed into one meaningful
commit that neatly expresses what it does. Though rebase might seem scary at
first, it's not something that can't be undone. Git's [`reflog`](http://git-scm.com/docs/git-reflog) command is always there for you when you want to recover specific commits that were squashed, or even undo a rebase altogether.

The goal of these commits is to create separate, succinct statements
about our project's progress. Just like we apply the Single Responsibility Principle to our code, we should also
apply it our commits. They should clearly do and express one thing only.

**NOTE**:

***Do not*** use rebase on commits that have been published already and others are using. It will cause serious issues with the repositories of the people who pulled down the changes prior to the rebase. Our lives as developers are already complicated enough, so there's no need to complicate them even further. The only exception to this rule is if a team has discussed and collectively agreed upon this course of action.

## Settle On A Convention

One of the most important things to always keep in mind is the fact that
our code will be read by others. With that in mind, it is important that
we strive to make our intentions as clear as possible. Even if it's a
personal project that only you have access to, meaningful commit messages will
come in handy when you return to that code in a few days’/weeks’/months’
time.

A while back, I read Tim Pope's blog [post](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) on the subject, and have since been following similar guidelines myself. One of the steps I took was to start using `git commit` instead of the standard pattern of `git commit -m "some message"`, which makes it easy to write short and non-descriptive messages. To make this process easier, I created a short alias `gc` for `git commit`, which opens a new template for writing commit messages that can be longer and more descriptive. Additionally, I created a custom `commit.template` to replace Git's default template. To achieve this, you need to first create a template, which is simply a **.txt** file. The one I currently use looks like this:

{% highlight bash %}
# Title (concise, 50 characters or fewer, imperative language — "Fetch" vs "Fetches")

# Body (detailed explanation)
# number (bug/issue/tracking if applicable)
{% endhighlight bash %}
Once created, you need to tell Git to use that instead of the default:

{% highlight bash %}
git config --global commit.template /path/to/git-commit-template.txt
{% endhighlight bash %}

The key point here is communication. Communicating with your team and agreeing on what makes a good commit message, as well as establishing a convention, will pay off in the future and make things easier for everyone on the project.

## Always Stay Green

This is crucial. Every commit should pass the tests. Running the tests
before committing is always best practice, but we are human and we make
mistakes (yes, not running the tests before a commit is a mistake). Committing early
and often, as described above, can also lead to this trap; but even your **WIP**
commits should satisfy this condition. If commit B depends upon commit A
to pass the tests, then what you have in essence is [temporal
coupling](http://blog.ploeh.dk/2011/05/24/DesignSmellTemporalCoupling/).
Fortunately, the solution here is simple: squash them together using
rebase.

Any commit that will be made public and that someone can revert to at any
given point should pass all the tests. Anyone should feel comfortable reverting to a specific commit without having to worry that it will break the tests.

## Be Thorough

I'm reviewing an important pull request, and Travis tells me that the new code passes the tests and builds successfully. Do I merge? Maybe. Sometimes my next step would be to pull down the branch and run it locally to verify that all the intended functionality works as advertised. If the PR is small and does not introduce significant changes to the codebase, then this step is unnecessary. I have encountered situations, though, where features that worked on the machine of the person who submitted the PR did not work on mine, and vice versa. Usually, local configuration/environment is the culprit, so it's always a safe extra check.

Once the branch is merged and the feature is considered complete, I will delete it. GitHub makes it easy, since it displays the option straight away after the merge. It's also very easy to perform the action from the command line. On older versions of Git, the command was:

{% highlight bash %}
git push origin :feature-branch
{% endhighlight bash %}

Since Git 1.7.0, it has changed to the more intuitive:

{% highlight bash %}
git push origin --delete feature-branch
{% endhighlight bash %}

Once the branches are deleted from the remote repository, I can also clean up my local references to those branches with:

{% highlight bash %}
git remote prune origin

Pruning origin
URL: git@github.com:account/project-name
 * [pruned] origin/feature-branch
{% endhighlight bash %}

Fewer branches means fewer things to worry about and less likelihood of things going out of control. When looking at a project, there aren’t many things that will make you feel more helpless than the sight of hundreds of branches. Keep it clean, keep it simple.

## Be Thoughtful

The ideas expressed above are simply guidelines and certainly not hard
rules. Some of them might not be fully under your control (convention
followed for commit messages), but others are (committing early).

One thing that is definitely under our control as developers and individuals is
discipline. Being disciplined alongside the actions we take will affect not
just those who work with us, but also our future selves. With that in
mind, lets try and make things better for them all.

---
layout: post
title: "Git and Zsh tips"
category: blog
tag:
- git
- zsh
---

Yesterday I published my first screencast where I performed the Roman Numerals kata. At the beginning of the screencast I typed the command `tpipe` on the bottom panel which essentially opened a named pipe. I describe most of the process [here](http://maikon.github.io/2014/06/19/quicker-testing.html) so I won't go much into details.

What I will show is the little commands I added to my `.zshrc` file that provides me with the above command:

    function tpipe () {
      mkfifo test-commands
      echo 'while true; do sh -c "$(cat test-commands)"; done' > run-test.sh
      sh run-test.sh
    }

So when I call the command in a pane it:

- creates named pane called test-commands (line 1)
- write the loop containing the command that keeps the reading end open in a file called run-test (line 2)
- execute the file (line 3)

In my .vimrc file I have the following line that maps \o keystroke and runs all the tests:

`nnoremap <silent> \o :w\|: !echo "clear; rspec $(grep . -l *_spec.rb)" > test-commands<cr>`

I'm sure there's a better (more clever) way of doing this but since I accepted the first thing that made sense to me when I was messing around and trying to get it to work.

##### Git log

For a while now I've been meaning to create a custom function that displays my commit log in a nicer format than the standard `git log`. After playing around for few mins and using the guidelines [here](http://git-scm.com/docs/git-log) I put together this:

    function gitl () {
      git log -$1 --pretty=format:'%Cred %aD | %H |%Cgreen %an %Cblue| %s' | cat
    }

I would attach a picture to show the output but didn't get around to confiure my blog in order to take images but will update when doing so. To see the result though you can simply copy the line and paste it into an existing project and replace the -$1 part with number of commits you want to see.

Another few example aliases I have are the below:

`alias -g gitc='git diff --cached'`

`alias -g gitd='git diff'`

`alias -g gits='git status'`

Even though I've only recently put them up, you can find my .dotfiles [here](https://github.com/Maikon/.dotfiles) for more info.

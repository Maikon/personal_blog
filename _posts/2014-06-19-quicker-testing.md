---
layout: post
title: 'Quicker Testing with Vim and Shell script'
category: blog
tag:
- rspec
- ruby
- tdd
- vim
- unix
---

One of the tasks for my new iteration is to perform, record and publish the roman numerals kata, in under 10 minutes. I've done the kata quite a few times but never actually try to get it under 10. I did perform it yesterday in front of the team and we clocked it at 20 but a big part of the process was getting the right setup in place.

The way I usually work with Vim I have two panes, one with the tests and another one with the code I'm testing. Being a fan of [Gary Bernhardt](https://twitter.com/garybernhardt), I took some pieces from his [.dotfiles](https://github.com/garybernhardt/dotfiles) and more specifically his [.vimrc](https://github.com/garybernhardt/dotfiles/blob/master/.vimrc) file. I would highly recommend the screencast series he made at [DAS](https://www.destroyallsoftware.com/screencasts) for some really great stuff.

The above is a good setup when developing solo but when giving a presentation it's ideal to have a third pane where the tests run. When doing Rails you can automate the process using a gem like guard but it would be quite an overkill to put it mildly, having to install a gem for a kata. After watching [this](https://www.destroyallsoftware.com/screencasts/catalog/running-tests-asynchronously) screencast by Gary, I saw how he emulated a similar scenario where he created a [FIFO](https://en.wikipedia.org/wiki/Named_pipe) and having that running on one pane, he sent input to it through a separate pane.

To make a FIFO you simply type the following in the terminal: `mkfifo pipe_name`. Once you create one you can open the reading end of it using `cat pipe_name`. Opening up a new pane, you can send a test message by doing the following `echo 'hello' > pipe_name` and you should see the result on the other side. What he did in the end was to have a small script like this which would keep the reading end open so he could continuously send commands. This is the little shell script he wrote:

   while true do
     sh -c "$(cat test-commands)"
   done

By having the above he could could sent a command to the `test-commands` pipe and run the tests like so:

`echo "rspec %" > test-commands`

This will run the tests in the current file and output the result at the reading end of the pipe.

After playing around a bit I made this small function in my .zshrc file so that I can use it whenever I need it:

  function tpipe () {
    mkfifo testing
    echo 'while true; do sh -c "$(cat testing)"; done' > run-test.sh
    sh run-test.sh
  }

So now when I open a new pane and simply type `tpipe` it will:

- create the FIFO `mkfifo testing`
- write `'while true; do sh -c "$(cat testing)"; done'` to the file `run-test.sh`
- run the file with `sh` to open reading end

After doing that I mapped a key like Gary did but with a small twist in my .vimrc file like so:

`nnoremap <silent> ,rt :w\|: !echo "clear; rspec %" > test-commands<cr>`

I added the `clear` command so that it will clear the screen before running the tests just to avoid any confusion with previous results.

I'll publish the screencast here when I'm done with it and you'll see the above in action just to get an idea of how it works better. Of course since everything is about improving, this could do with some fine tuning but so far I'm pretty happy with the result.

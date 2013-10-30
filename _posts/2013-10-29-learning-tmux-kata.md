---
layout: post
title: Learning tmux kata style
---

Here are my notes on how I'm getting started with tmux.

I like [kata](http://en.wikipedia.org/wiki/Kata) style of learning - "detailed choreographed patterns of movements practiced solo or in pairs". Why? "The goal is to internalize the movements and techniques of a kata so they can be executed and adapted under different circumstances, without thought or hesitation. A novice’s actions will look uneven and difficult, while a master’s appear simple and smooth"

As a novice then I often start with uneven, difficult, awkward, unsure, discombobulated patterns of movements and as I pracice the movements I grow my capacity to execute them without hesitation. For me learning is mechanical, a motor skill mostly.

## Learning Environment.

I am on a Mac Book Air with [homebrew](http://brew.sh/) and [brew cask](https://github.com/phinze/homebrew-cask). With this setup I `brew install tmux` and `brew cask install iterm2`. On linux machine I usualy use Mint so gnome-terminal and tmux with sudo apt-get

Done. Ready to quick start. Here are my instructions for learning.

## Kata: tmux quick start (45 minutes)

Watch [tmux Quick Start Video](https://www.youtube.com/watch?v=wKEGA8oEWXw) from [@geeksam](http://twitter.com/geeksam). it's about 10 minutes. I watched it first to get the whole picutre.

OK, I watch it again. This time I notice new things I didn't see the first time. I imaging myself typing at the terminal all the commands Sam is mentioning in his screencast. I try to build a mental model of Session, Window, creating Windows, Panes, detaching, attaching, exiting.

OK, I watch it again (yes, third time). This time I try to stop the video and try out the things Sam mentions. I am following along mimicking all the moves he makes during the screencast.

Done.

## Kata: tmux manual (15 min)

Viewing tmux man pages. Open terminal and execute `man tmux`. I output the manual to file with `man -t tmux > ~/Documents/tmux.ps` and open it. Preview on mac converts ps to pdf. I save it as pdf for quick look as I study tmux. Help is about 33 pages of info on my machine.  I just peruse the sections, not to worry, I don't understand eveyrhing. Just letting my eyes get used to sections. As I view sections I remember concepts from the quick start video.

Done.

## Kata: tmux pragprog book (2 hours)

There is a great [Tmux pragprog book](http://pragprog.com/book/bhtmux/tmux). I got [this tmux book on amazon kindle for $7](http://www.amazon.com/tmux-Productive-Mouse-Free-Development-ebook/dp/B00A4I3ZVY/ref=sr_1_1_bnp_1_kin?ie=UTF8&qid=1383079628&sr=8-1&keywords=tmux).

Kindle tells me I have 1584 pages. I started with chapter 1.2 page 209 and ended with chapter 4.5. That's about 1000 pages with 10 seconds per page. About 2 hours.

I am reading the book quickly from the beginning to the end. Not too worry about retaining all the details. Just getting more solid info on starting session, attaching, detaching, windows, panes.

Done.

## Kata: tmux sessions with one window (15 min)

Trying out deliberate work with tmux sessions. By default tmux starts session with one 'window'.

### Context: When I am in terminal and I want to get into tmux

`tmux` start new session with an autoincremented number and one window identifed as `'0'`

`tmux new -s foofoo` starts new session named `foofoo`

`tmux attach -t foofoo` attach to -target session named `foofoo` previously started and detached from

`tmux attach -t 3` attach to -target auto numbered session `3` previously started and detached from


### Context: When I am in terminal I want to manage tmux sessions

`tmux ls` list-sessions shows by name or number

`tmux kill-session -t foofoo` kill target foofoof session. gone.

`tmux kill-session -t 3` kill the autonamed session identified by number `3`. gone


### Context: When I am in tmux session and I want to get back to terminal

`exit` kills current session, back to terminal. session is gone.

`C-b d` detach from current session and back to terminal. session is preserved and I can reattach to it.


## Kata: tmux creating new windows in tmux (15 min)

`tmux new -s foo` starts session foo and gives me one window by default. Identified as `'0'` in status bar. The window is not named. it says `'bash'` by default.

`C-b c` creates new window with number `'1'`. Now I have 2 windows. Number '1' has `*` after it's name indicating current window. Number '0' has `-` after it's name indicating previous window.

`C-b w` windows list shows me two windows 0 and 1. Windows are numbered by default and have 'bash' as default name followed by 'hostname' of the machine I am on.

`C-b c C-b c C-b c` create three more windows `'2'`, `'3'`, `'4'`

`C-b w` windows list, use j or k vim shortcuts to move down and up in the list. Press enter to select one. See where `*` and `-` indicators are set for current and previous window.

### moving between windows.

Current window has `*` indicator, previous windows has `-` indicator.

`C-b p` previous window. notice `*` and `-` indicators move

`C-b n` next window. round and round it goes.

`C-b 0` window number `0`. using number to select window.

`C-b 77` window number `77`, and since there is no such a winddow tmux tells me `not found`. OK. fair.

### rename window

`C-b ,` prompts rename-window. type `foo`. press enter. Rename some more windows the same way.

`C-b f` prompts to find window by name, type first letters and enter.

`C-b w` windows list showing now all the new names.

`exit` kills the current window. and if it's the last window kill tmux session as well.

### kill windows but not session

`C-b &` prompts to kill current window with all it's panes opened.


## Kata: Window panes (15 min)

Each window you can split into 'panes'.

`C-b %` yes, that means `Shift+5`. divide window | way, we'll fix it in `.tmux.conf` file

`C-b "` yes, that means `Shift+'`. divide window -- way. we'll fix in `.tmux.conf` file.

`C-b o` (o)pening, move focus to next opened pane in a window

`C-b <arrow key>` up, down, left, right opened pane.

`C-b <space>` cycle 5 builtin layouts for panes. Open 3 panes and try it.

`C-b x` prompts to kill current pane, yes/no?

`C-b q` query all open panes, display briefly number assinged to the pane and its dimension.

`exit` just exit the current pane. if the last pane it exists the 'window' and if the last window it exits the session. so it goes.


## Kata: Command mode

`C-b :` enter command mode like in vim. you can send commands to tmux.

`C-b ?` shows 44 bind-key entries. q quits the view and returns to window.


## Update Tmux Kata Day 2

And we are swithing all around a bit. Some changes.


### tmux.conf file

I created `~/.tmux.conf` file based on reading tmux book

- PREFIX set to C-a

I have abandoned the default `C-b`. I have switched to `C-a` and I've been training my fingers to do that today.

My setting for PREFIX is this in `~/.tmux.conf`

```
# PREFIX
unbind C-b
set -g prefix 'C-a'
bind 'C-a' send-prefix
```

I am leaving C-b alone for working in commandline to move back one char (emacs default mode).

- mouse off

Next I turned mouse off with `set -g mode-mouse off`. Let's see how this goes.

- Split Panes

I don't like recommended way of splitting panes `C-a |` with `bind | split-window -h`. This requires to use Shift key. I chose `bind /' instead and may revert if I find conflicts.

### Kata: study ls-pair (20 min)

Studied [ls-pair projec](https://github.com/livingsocial/ls-pair). Will need to install hamachi and learn wemux a bit.

Done.

### Kata: read about Tmuxinator (15 min)

`gem install tmuxinator`. Tmuxinator allows you to setup configuations for sessions, how many windows, what is loade in windows. How to prepare windows and panes. sounds like a nice configurator.

Done.

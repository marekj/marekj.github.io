---
layout: post
title: Setup tmux pair user
---

[This LivingSocial remote pairing project ls-pair](https://github.com/livingsocial/ls-pair) seems like a good place to start setting up machines for remote pair programming.

I will do quick setup where I will use one machine. I will create a secondary user on my machine named `tmuxpair` ssh as that user on my local machine and finally have the two users in tmux session pairing. This will show me that using wemux and the ls-pair setups can work for me.

Let's get started with reading [howto host a session](https://github.com/livingsocial/ls-pair#howto-host-a-session-on-your-computer).

First I `git clone git@github.com:livingsocial/ls-pair.git` to my git projects dir and cd to that directory. This is where I'll run all my commands. I see there is a `bin/install` but I will not use it yet. I want to roll my configuration manually to understand all the moving parts first before automating.

Let's call this "Iteration 0". Create a `tmuxpair` user on my machine and pair with myself on two diff accounts on the same machine. When I go throught this excercise and get to know the moving parts I will pair from another machine on my network in "Iteration 1".


## tmuxpair user ssh keys and account on my machine.

I plan on creating a user account on my machine named `tmuxpair`. [I create an ssh key](https://help.github.com/articles/generating-ssh-keys) and name the keys as tmuxpair and tmuxpair.pub - and I move the keys to `ls-pair/public_keys` directory. With ssh keys there (well, you only need the .pub) I run `sudo bin/create_pair_account` and when asked for user to create I supply `tmuxpair` username. The script creates a standard user account on my mac named `tmuxpair Pair Account` and moves the ssh key from `public_keys` to newly created `/Users/tmuxpair/.ssh/authorized_keys`.

Done.

## Wemux

We are going to need [wemux](https://github.com/zolrath/wemux). `brew install wemux` and `wemux start` starts a tmux session named wemux. In the status bar I see my username displayed as the person who joined wemux session.

Done.

## tmuxpair user joins the session.

while being in ls-pair directory `ssh tmuxpair@hostname -i public_keys/tmuxpair` (yes, I am using the private key I have created earlier for this Iteration 0 simulation) - and I am now logged in becuase the corresponding public key was copied to authorized_keys on my account.

While being `tmuxpair` on ssh I execute `wemux pair` and boom, I am in the same wemux session. Victory.

Done.

## wemux pairs

As a user `tmuxpair` in wemux session I am in a pair mode. When I type `whoami` it displays `rubytester` so in effect my pair has all the ownership of files as I do on my end of wemux (isn't that a bit strange? or it should be this way)

`wemux users` shows me two users: rubytester and tmuxpair

To be contiued...

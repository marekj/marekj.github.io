---
layout: post
title: Setup tmux remote pair user
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


# Iternation 1: Allowing ssh from the outside.

Next I want to connect from outside of my machine. Let's look at hamachi, ngrok and localtunnel gem.

## Hamachi

Install hamachi `brew cask install hamachi`. Open the installer app to install hamachi. After starting I named hamachi client with my machine name and created ad hoc network named tmuxpair.

On my linux box I [installed hamachi and haguichi as described in ubuntu help.](https://help.ubuntu.com/community/Hamachi). Started haguichi, named client with machine name and joined network named tmuxpair (here you will need to provide your pair with a password for the network, I also think hamachi allows to somehow accept clients wihout passwords but I need to check it)

OK, VPN client running on both machines. On my mac I can see the other machine joined and on my linux box I seen see my mac's IP provided by hamachi. I will use that to ssh to the machine as tmuxpair user but first I need to provide my linux machine private key for tmuxpair user `ssh tmuxpair@XX.XX.XX.XX` and once there I `wemux pair`.

Done.

## ngrok

In order to use ngrok ssh tunnel I need to [sign up for new account on ngrok.com](http://ngrok.com/). If I end up liking it I should pay for it. I just found out about it thanks to [the post about pairing from @samuelgoodwin](http://samuelgoodwin.tumblr.com/post/64769827818/how-to-get-started-with-remote-pairing-quickly)

After signing up I got an auth token. Downloaded ngrok.zip, unziped to `~/bin/ngrok` and started the tunnel with: `ngrok --authtoken <mytoken> -proto=tcp 22` - ngrok gave me a tcp forward connection on grok.com and 5 digit port number, for example: ngrok.com:12345

Just as before when I tested with tmuxpair I cd to ls-pair directory and connect to my machine using ngrok's tunnel. `ssh tmuxpair@ngrok -p 12345 -i public_keys/tmuxpair` and I am prompted for tmuxpair passphrase.

After connecting I `wemux pair` and I am connected to the tmux session as before.

Done.


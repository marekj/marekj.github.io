--- 
title: Moving to GVim on Windows and Ubuntu
date: 2009-09-13 16:03:35.593020 -05:00
layout: post
---

## Getting to know Vim. Nobody has ever told me how simple it is.

The first person I've ever met who uses vim was Charley Baker. He actually showed me Vim running on his Mac at AWTA2009 in Austin.
It's funny that as a tester working with developers for many years I have never met anyone working with Vim
and most of the 'reputation' I hear about vim is that 'it's very hard' to learn to 'understand' it and to learn to 'operate' it.

So I asked some questions and poked around and found out it's not so hard. It is a great editor for a tester.
It seems there is no 'magic'. There is only 'practice' just like everthing else. Well, easier said than done of course.

I am writing this in 'Gvim' on Ubuntu (vim-genome package, no sure what the diff is with gtk package).
I also set up 'Gvim' on my WinXP machine. The first thing conceptually I had to understand was
that Vim is a 'tool for working with text' environment, you 'operate' on text in vim.
It's nice that vim is all about making 'you operate' on text, I mean really 'you'.
I am just starting to learn. The two things to understand are config files `_vimrc`, `_gvimrc` and vimfiles on windows
and for ubutntu the `.vim` dir and `.vimrc`, `.gvimrc` files. They are mostly the same. (FYI plenty of resources on github)

## Gvim on Windows

I downloaded Gvim on Windows and installed it into `c:\Programs\vim`.
In Windows the `~` home dir is `%USERPROFILE%` so that's where I put my `_vimrc` and `_gvimrc`.
The `_gvimrc` file is read by gvim only, not by vim (correct?).
If I want to use vim in a commandline I would not need `_gvimrc` (still learnig on this one).
The next thing was to create `~/vimfiles/colors/` and put in there a `herald.vim` file. Nice colorscheme. There are more. Many many more.

On Ubuntu the similar thing was to create some settings in `~/.vimrc` and `~/.gvimrc` which are
equivalent to `_vimrc` and `_gvimrc` on windows and `~/vim/` directory where you keep colors and plugins; equivalent to `~/vimfiles` on windows.

Gvim out of the box is ugly, maybe this is why most people give up on it in the first few minutes.
I use Bitstream Vera Sans Mono font. (been using this font for ever it seems).

I think I am on my way to give up notepad++, jedit, scite and stick with vim.
I am glad I've just decided to take a day and poke around to see if I can 'get vim' on my own.
But really I am quite surprised that nobody has showed me 'vim' before.

As to IDE I am sticking with Netbeans for now but for all text stuff I am looking forward to learning more 'vim' hacks. 

P.S. Well, this post is a bit lame. I am just practicing on vim here. 

---
layout: post
title: git log aliases for quick look at git history
---

To display `git log` information and scan the recent history of a repo I use 4 aliases, each distinct, each giving me a different view. Here is a snippet of my `~/.gitconfig` where I define them.

```
[alias]
  ;log graph files and changes
  l = log --stat --abbrev-commit --pretty=oneline --graph -t  --cc

  ;log graph all branches
  h = log --oneline --abbrev-commit --branches=* --graph --decorate

  ;log patch hunk minimum info
  lp =  log --unified=3 --abbrev-commit --date=relative

  ;log patch view color words instead of entire lines
  lpw = log --unified=1 --abbrev-commit --date=relative --color-words
```

I don't like to type `git` commmand so I have an have `alias g="git"` in my profile. For example I type `g h` for a quick look rather than `git h` which as you see resolves to a full command `git log --oneline --abbrev-commit --branches=* --graph --decorate`. (Aliases FTW! ladies and gentelment)

How do I use those 4 log views?. Here is brief demo and my reasons for their constructions.

![Animated Gif of git log aliases in action](/images/git-log-alias-rubytester.gif)

## g l

Use `g l` for `git log --stat --abbrev-commit --pretty=oneline --graph -t  --cc`

This view allows me to quickly see how the size of changes I am dealing with as well as the relative size of change to other commits. I can quickly scan the history of the last 100 commits visually to adjust my filter and determine what complexity of changes occured.

Each commit is showing 3 parts of information: First line is 7 chars of commit's sha1 followed by a commit headline. Right after the list of files that changed with the change count, the green + visualizes insertions, the red deletions. The last part is a summary. Count of files changes, cumulative count for insertsions and deltions.

## g h

Use `g h` for `git log --oneline --abbrev-commit --branches=* --graph --decorate`

This is also a graph but I don't care about the size of each commit like I do in `g l`. I want to see all the branches and tags. Why? This view gives me some meaning around stopping points in the recent history. It's about graphs and labels on nodes showing me divergence and convergence. This view shows paths of travel and stops along the way, a heartbeat of a journey through codebase.

## g lp

Use `g lp` for `git log --unified=1 --abbrev-commit --date=relative`

This view is a dive into each commit. Now I examine the commit itself using relative time of change and a list of hunks. The freshness of change is important, if I want to ask a question about the change I can be brief if the change occured recently (Eeasy to ask about something changed '5 minutes ago' than `5 days ago`)

I scan the list of `hunks` (usually with 3 lines of context surrounding the change, however for codebase I work with often the context is 1). I can scan visually the size of hunks affected in each commit.

## g lpw

Use `g lpw` for `git log --unified=0 --abbrev-commit --date=relative --color-words`

This is a variant on `g lp`. This view shows only words changed in hunks and no context (or just 1 line of insiste on better context comprehension). Scan quickly the last 10 to 100 commits only for word changes. Perhaps a method name changes to add clarity? this view gives me a nice view. Many times this is the one I use right away to see the last few changes to quikly scan the intention of commits.

## Conclusion

I use 4 git log aliases to quickly comprehend the recent changes to a repo I am working with.

- `g l` for size of chagnes
- `g h` for paths, labels on recent journey.
- `g lp` for hunks
- `g lpq` for scanning words in hunks

Each git log alias allows me to view a different part of repo. Each view matters to me to build a comprehension of recent changes.

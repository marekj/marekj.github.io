---
layout: post
title: git log aliases for quick look at git history
---

I use 4 git log aliases to quickly inspect the recent git history of a current repo. Here is a snippet of my `~/.gitconfig`.

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

I alos have `alias g="git"` so in terminal I just type `g h` for a quick look rather than `git h` which obviously resolves to a full command `git log --oneline --abbrev-commit --branches=* --graph --decorate`

How do I use those 4 log aliases?. Here is brief demo and my usage of it.

![Animated Gif of git log aliases in action](/images/git-log-alias-rubytester.jpg)

### g l

Use `g l` for `git log --stat --abbrev-commit --pretty=oneline --graph -t  --cc`

It displays a graph of commits and their parent commits for file stats.

Each commit is showing 3 parts of information: First line is 7 chars of commit's sha1 followed by a commit headline. Right after the list of files that changed with the change count, the green + visualizes insertions, the red deletions. The last part is a summary. Count of files changes, cumulative count for insertsions and deltions.

This view allows me to quickly see how the size of changes I am dealing with as well as the relative size of change to other commits. I can quickly scan the history of the last 100 commits visually to adjust my filter and determine what complexity of changes occured.

### g h

Use `g h` for `git log --oneline --abbrev-commit --branches=* --graph --decorate`

This is also a graph but I don't care about the size of each commit like I do in `g l`. I want to see all the branches and tags. This view gives me some meaning around stopping points in the history. It's just about graphing and labels on nodes showing me divergence and convergence. This view shows paths of travel and stops along the way, a heartbeat of a journey with codebase.

### g lp

Use `g lp` for `git log --unified=1 --abbrev-commit --date=relative`

Now I can examine the commit itself using relative time of change and a list of hunks. The freshness of change is important, if I want to ask a question about the change I can be brief if the change occured recently (Eeasy to ask about something changed '5 minutes ago' than `5 days ago`)

Next comes the list of `hunks` to examine with 3 lines of context surrounding the change (most of the time I have it set to 1 if I work with codebase often). I can scan visually the size of hunks affected in a change.

### g lpw

Use `g lpw` for `git log --unified=0 --abbrev-commit --date=relative --color-words`

This is a simplified variant on `g lp`. This view shows only words changed in hunks and no context (or just 1 line of insiste on better context comprehension).


## Conclusion

I use 4 git log aliases to quickly comprehend the recent changes to a repo I am working with.

- `g l` for size of chagnes
- `g h` for paths, labels on recent journey.
- `g lp` for hunks
- `g lpq` for scanning words in hunks

Each git log alias allows me to view a different part of repo. Each view matters to me to build a comprehension of recent changes.



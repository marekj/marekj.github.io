---
layout: post
title: git log aliases for quick look at git history
---

To display `git log` information and scan the recent history of a repo I use 4 aliases, each giving me a distinct view. Here is a snippet of my `~/.gitconfig` where I define them.

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

I don't like to type `git` commmand so I have an `alias g="git"` in my `~/.bashrc`. For example I type `g h` rather than `git h` which as you see resolves to a full command `git log --oneline --abbrev-commit --branches=* --graph --decorate`. (Aliases FTW! ladies and gentelment)

How do I use those 4 log views? Here is brief demo and my reasons for their constructions.

![Animated Gif of git log aliases in action](/images/git-log-alias-rubytester.gif)

## g l

Use `g l` for `git log --stat --abbrev-commit --pretty=oneline --graph -t  --cc`

This view allows me to quickly see how the size of changes I am dealing with as well as the size of a change relative to other commits. I can quickly scan the history of the last 100 commits visually to and determine complexity of changes which occured.

In this view each commit is showing 3 parts of information: First line: 7 chars of commit's sha1 followed by a commit headline. Right after the changed list with their change count, the green + visualizes insertions, the red - deletions. The last part is a summary: count of files changes with cumulative count for insertsions and deltions.

## g h

Use `g h` for `git log --oneline --abbrev-commit --branches=* --graph --decorate`

This is also a graph but I this time I don't want to see the size of each commit like I do in `g l`. I want to see all the branches and tags. Why? This view gives me some meaning around stopping points in the recent history of commits. It's about graphs and labels on nodes showing divergence and convergence. This view shows travel paths and stops along the way, a heartbeat of a journey through codebase.

## g lp

Use `g lp` for `git log --unified=1 --abbrev-commit --date=relative`

With this view I dive into each commit. I examine the commit itself using relative time of change and a list of hunks. The freshness of change is important, if I want to ask a question about the change I can be brief if the change occured recently (eeasier to ask about something that changed `5 minutes ago` than `5 days ago`)

I scan the list of diff `hunks` (usually with 3 lines of context surrounding the change, and for codebase I work with often the context set to 1). I visualize the size of change by hunks affected in each commit.

## g lpw

Use `g lpw` for `git log --unified=0 --abbrev-commit --date=relative --color-words`

This is a variant on `g lp`. This view shows only words changed in hunks and no context (or just 1 line to increase comprehension). Scanning quickly the last 10 or 100 commits only for word changes lets me glean the essence of a change; perhaps a method name changed to add clarity? typos? Many times this is the view I use right away to see the last few commits to gain familiarity and estblish context of what's current and what may be next.

## Conclusion

I use 4 git log aliases to quickly comprehend the recent changes to a repo I am working with.

- `g l` for size of changes
- `g h` for paths, labels, tags, merges.
- `g lp` for hunks and busyness of each change
- `g lpq` for scanning words in hunks to establish recent context.

Each git log alias allows me to have a distinct view of repo's history. Each lets me visualize and comprehend recent changes so I can participate in a codebase gardening.

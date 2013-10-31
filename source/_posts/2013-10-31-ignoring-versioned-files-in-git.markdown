---
layout: post
title: "Ignoring Versioned Files In git"
date: 2013-10-31 14:09
comments: true
categories: [git, programming]
---

Sometimes you work on a file that almost never changes but is used for configuration purposes. 

Each time you make an update to that file, it shows up in your *changes to be committed* area

```bash
git st
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
# modified:   my.conf
```

By running the command `git update-index --assume-unchanged path/to/my.conf` the file will logically 
be assumed unchanged and will never show up in your *changes to be committed* area.

```bash
git update-index --assume-unchanged my.conf
git st
# On branch master
nothing to commit, working directory clean
```

The reverse operation is `git update-index --no-assume-unchanged path/to/file.txt`
```bash
git update-index --no-assume-unchanged my.conf
git st
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
# modified:   my.conf
```



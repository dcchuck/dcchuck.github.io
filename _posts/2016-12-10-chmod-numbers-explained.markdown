---
layout: post
title:  "chmod Numbers Explained"
date:   2016-12-10 12:00:00 -0400
categories: linux
---

Today marks my 3rd time in 2016 googling what number I need to send `chmod` in order to get the permissions correct for an ssh key. In the interest of never doing that again, let's see what those numbers mean.

`chmod` takes a 3 digit number. Each of those digits represents a file permission code. The code is as follows:

```
0 == --- == no access
1 == --x == execute
2 == -w- == write
3 == -wx == write / execute
4 == r-- == read
5 == r-x == read / execute
6 == rw- == read / write
7 == rwx == read / write / execute
```

There are 3 - one for the file owner, one for group permissions, and one for world permissions - just like you'd see when you `ls -al`.

So what permissions are necessary for a private .ssh key? Only the file owner should have read access. Looking at our code we'll need `chmod 400`.

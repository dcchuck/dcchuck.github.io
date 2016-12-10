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

```
> ls -al chmod_examples
total 0
drwxr-xr-x   5 chuck  staff   170 Dec 10 17:55 .
drwxr-xr-x+ 83 chuck  staff  2822 Dec 10 17:57 ..
-r--------   1 chuck  staff     0 Dec 10 17:55 a_private_key.pem
-r-xr-x---   1 chuck  staff     0 Dec 10 17:55 best_bash_script_ever.sh
-rw-r--r--   1 chuck  staff     0 Dec 10 17:55 whatever
```

So what permissions are necessary for a private .ssh key? Only the file owner should have read access. Looking at our code we'll need `chmod 400`. After running chmod 400 the ssh key file permissions should match those in the printout above.

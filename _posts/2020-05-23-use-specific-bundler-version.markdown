---
layout: post
title:  "Use A Specific Version of Bundler When Running Commands"
date:   2020-05-23 12:00:00 -0400
categories: ruby
---

```bash
$ bundle --version
Bundler version 1.17.3

# Pass in the version you wish to use, surrounded by underscores (_)

$ bundle _2.1.4_ --version
Bundler version 2.1.4
```

# Caveats

Multiple major versions of bundler will collide. Keep only one installation of each major version (One bundler `1.*`, one bundler `2.*`).

To verify the installed versions of bundler

```
$ gem list bundler

bundler (default: 2.1.4, 1.17.3)
```

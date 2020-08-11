---
layout: post
title:  "Clean Branch History with --fixup"
date:   2020-08-10 12:00:00 -0400
categories: git, source control, tools
---

A common worflow when part of a development team might be:

* Create a new feature branch off of `main` (formerly `master`) - `git checkout -b my-feature`
* Make several commits to the branch
* Create a pull request
* Get reviewed, consume feedback
* Resubmit for review

When you `make several commits to the branch` we can assume that these commits are separated for a reason. Each commit makes a new change to the project and a direct child of `HEAD` (usually the tip of the current branch). Ideally, I would not have a commit in my git history with a message like "PR Feedback".

## `--fixup`

Enter fixup commits. Let's take a look at a sample git history from the flow above.

```
$ git log --oneline
e236851fc (HEAD -> my-feature) Enforce discount code expiration on checkout
4f294a478 Accept multiple discount codes on checkout
aba81bdb8 Add multiple discount options to products model
```

Now after opening a PR, let's say get some feedback for cosmetic changes in any (or all!) of the above commits. With the `--fixup` flag you can make and commit a change which git will apply to that commit. Continuing from above.

```
# For reference
$ git log --oneline
e236851fc (HEAD -> my-feature) Enforce discount code expiration on checkout
4f294a478 Accept multiple discount codes on checkout
aba81bdb8 Add multiple discount options to products model
# Assume the linter caught something we wrote in commit aba81bdb8
$ git add (your changes)
$ git commit --fixup aba81bdb8
```

Now take a look at your commit history again

```
$ git log --oneline
e236851fc (HEAD -> my-feature) fixup! Add multiple discount options to products model
e236851fc Enforce discount code expiration on checkout
4f294a478 Accept multiple discount codes on checkout
aba81bdb8 Add multiple discount options to products model
```

Finally, to have git apply those changes directly to the commit it fixed and squash down the history you need only

```
$ git rebase --interactive --autosquash main
# Your editor should prompt here, but git will have populated the document already so you can write/quit
$ git log --oneline
xf86u5ifc (HEAD -> my-feature) Enforce discount code expiration on checkout
zt294a478 Accept multiple discount codes on checkout
l9a81bdb8 Add multiple discount options to products model
```

Notice a few things
* When your text editor displayed during the rebase, the fixup commit should be ordered directly after the commit it is fixing
* The commit hashes have changed. This makes sense, history has been "reorganized" before being merged into main

## When/When Not To

For situations like the one above, this can help you tell the story of your branch and keep it sound/pickable. If my branch has multiple commits and there are minor changes to be made across several of them, I will fixup each individually.

If your work is going to be squahed down anyway, you can skip the above. I advise you to remove any "worthless" commit messages in the summary squah commit if being thorough.

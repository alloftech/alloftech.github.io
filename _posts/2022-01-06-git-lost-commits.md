---
layout: post
title: "I messed up my git history: retrieving your lost commits"
date: 2022-01-06
last_modified_at: 2022-01-06
cover_image: 2022-01-06-feature.jpg
author: Simon Dosda
categories: git tool
excerpt_separator: <!--more-->
---

> Photo by [Elisa Ventur](https://unsplash.com/@elisa_ventur).

When using git commands that rewrite a branch's history, like `rebase` or `reset`, you might end up losing some commits by mistake. 

But don't worry, as long as you committed your changes once, you should be able to get them back!

<!--more-->

Indeed, a magical git command can help us: [git reflog](https://git-scm.com/docs/git-reflog). 

Git keeps a trace of all your commit in reference logs: this is, for instance, what allows you to use commands like `git checkout my-branch@{two.week.ago}` to checkout on your branch as it was two weeks ago.

So if you just messed up with your branch, you can do something like `git checkout my-branch@{ten.minute.ago}`, otherwise, it can be more accurate to look directly at the reference logs entries with the `git reflog` command.

## Reading the reference logs

The format of the reference logs is as follows:

`commit hash` (`branch name`) `handle`: `action`: `action description`.

Here is an example of a git reflog output:
can be
```bash
0e60901 (HEAD -> feature-branch, main) HEAD@{0}: reset: moving to main
2b38384 HEAD@{1}: commit: feat: new feature
0e60901 (HEAD -> feature-branch, main) HEAD@{2}: checkout: moving from main to feature-branch
```

Here for instance we have:

- HEAD@{2} - creation of a new branch named `feature-branch`
- HEAD@{1} - add a commit with the message `feat: new feature`
- HEAD@{0} - reset on branch `main`

When I have reset my branch on `main`, I used the `--hard` option, so currently the changes from my commit `feat: new feature` are lost.

But thanks to the `reflog,` I can checkout on them or directly reset my head on it with the following command:

```bash
git reset --hard HEAD@{1} # or with the commit hash: git reset --hard 2b38384
```

Here we go, I got back my lost commit!

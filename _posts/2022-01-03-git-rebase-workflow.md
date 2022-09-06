---
layout: post
title: Get a clean git history with a rebase workflow
date: 2022-01-03
last_modified_at: 2022-01-03
cover_image: 2022-01-03-feature.jpg
author: Simon Dosda
categories: git tool
excerpt_separator: <!--more-->
---

When I started using git, I had little concern about my commit history. 
I only used git as a backup tool for my work without trying to have consistent and readable commits.

That's a common pitfall with git. I have often worked in teams where we would integrate all our commit iterations into our main branch, with commit messages as descriptive as `wip`, ending with a **hardly useful history**.

<!--more-->

Doing so strongly limits the benefits of using git. 
When we choose to enforce having a clean, readable history, composed of well-defined atomic commits on a single linear branch, the advantages of doing so greatly overcome the little extra time we spend to ensure everything we integrate to our main branch is clean:

- we can now **easily read the history** and understand what changes were made
- if there are some changes we don't understand, we can read all the **relevant associated changes in the same commit**
- if we have a problem with a deployment, we can **easily revert the last commit**

In this article, I will introduce the main rules to maintain a clean git history and show you how I personally apply them in my day-to-day development.

There is no right way to use git, so don't take anything as an absolute truth but rather as an inspiration to create your own workflow.

## Rules for a clean history

There are only three rules to enforce a clean git history:

- a commit should be atomic: it should contain **all and only** the changes required for a feature or a fix
- a commit should have a **clear, descriptive message**. I also use [conventional commits](https://www.conventionalcommits.org) format, which can be helpful when working on a versioned project
- a commit should be directly integrated to long-running branches, using the `rebase` functionality of git, to **avoid merge commits**.

This last one can seem surprising, as it is easy to use git without even knowing the `rebase` command.

What `rebase` does is that it "moves" your commits on top of the branch you are rebasing on.

The main drawback of using `rebase` is that it rewrites your branch history. Therefore it won't be consistent with your remote repository, and you will need to force push on it.

But it will allow you to have a linear commit history, whereas the `merge` command will create a new commit for each merge with the changes from the merged branch, resulting in a less readable history.

![git merge vs git rebase](/assets/images/2022-01-03-git-merge-rebase.png)

This workflow works well, no matter if you are **alone on a personal project or working in a big team**.

But if you can always deal with your mess alone, these rules will be helpful if they are enforced in a team.

## Before we start, a word on git aliases

I use several times a day the commands shown in this article. 
To be more efficient, I have set up aliases for most of them. 
In the below examples, I will use the full command and show you as a comment the version using my aliases.

If you want to know more about them, you can go to my article explaining my [git aliases for an efficient workflow]({% post_url 2021-04-13-git-aliases %}).

## Starting a new development

I usually start by creating a new branch from the latest main branch.

```bash
git checkout main  # git co main
git pull
git checkout -b my-branch  # git nb my-branch
```

NB: there are two main ways to create a new branch:

- `git branch my-branch` will create a new branch but stay on the current one, which is useful when you want to save your current branch before altering it
- `git checkout -b my-branch` is a checkout command with the `-b` option that will create the new branch before checking out on it.

At this point, we just created a new branch similar to the main one.

![git workflow create branch](/assets/images/2022-01-03-git-workflow-1.png)

## Working on the issue

I then start working and will usually create several commits, depending on the complexity of the development. This allows going back if I take a wrong direction, or change my mind at one point. I still usually try to break things into commits that make sense.

```bash
git commit --all -m 'feat: step 1'  # git c 'feat: step 1'
git git push --set-upstream origin/my-branch  # git p
...
git commit --all -m 'feat: stept 7'  # git c 'feat: stept 7'
git push  # git p
```

![git workflow create commits](/assets/images/2022-01-03-git-workflow-2.png)

But at the end of the feature development, I will rewrite my history to have only one clean commit; or several if it is more appropriate (for instance, if I also fixed something while working on a feature).

To do so, I usually use an interactive `rebase`.

```bash
git rebase -i HEAD~7  # git ri 7
```

The way interactive `rebase` works is that it displays the list of commits, with the action you can do on it. 
The main ones I use are:

- `pick` or `p`: the default, keep the commit as it is
- `reword` or `r`: allows you to rewrite the commit message in a next step
- `squash` or `s`: integrate the commit with the previous one, you will be asked to edit the message
- `fixup` or `f`: integrate the commit with the previous one while keeping its message
- `drop` or `d`: remove the commit. You can also get the same result by removing the entire line

![git workflow squash](/assets/images/2022-01-03-git-workflow-3.png)

Once the history has been rewritten, I need to force push to the remote repository.

```bash
git push --force  # git fp
```

## Rebasing your branch

Before pushing my changes for a review, I will usually rebase on the main branch so that my branch is up to date.

```bash
git fetch & git rebase origin/main  # git rom
```

This is needed only if new changes were integrated into the main branch. 
If any conflicts were created, this is when you will be asked to solve them.

![git workflow rebase](/assets/images/2022-01-03-git-workflow-4.png)

## Reviewing the changes before integration

I then open a merge/pull request in gitlab/github, and almost always find corrections to make.

If the changes I want to make are important, I will create new commits, but for small corrections I usually integrate them directly to the current commit by amending it.

```bash
git commit --amend & git push --force  # git afp
```

When everything seems clean, I will ask for a review and will usually integrate the changes requested in a new commit so that the reviewer can quickly check them.

Eventually, before integrating this new feature to the main branch, I will regroup the commits into one again as done previously, and rebase on the main branch if necessary.

**NB: For this workflow to be effective, you need to enforce rebase merging in gitlab/github.**

## Cleaning up

Finally, Once merged, I will delete my branch on the server (this is configured to be done automatically after merging) and on my local repository, so that these repositories stay clean.

```bash
git branch -D my-branch  # git del my-branch
```

I have automated the setup of all my projects [thanks to tmuxp]({% post_url 2021-05-03-tmux %}), and I always start by updating the repository and displaying its status. 
That allows me to quickly see what branches I am currently working on.

```bash
git fetch && git branch -vv && git status  # git state
```

## Wrapping up

These are the rules and the workflow I try to follow on all my projects.

I admit it requires some effort, but the time you spend making sure that your git history is clean and readable will save you a lot of time when you will need to understand why and how some changes were made.

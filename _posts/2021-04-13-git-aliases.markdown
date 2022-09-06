---
layout: post
title: Git Aliases for an Efficient Workflow
date: 2021-04-13
last_modified_at: 2022-01-14
cover_image: 2021-04-13-feature.png
author: Simon Dosda
categories: git tool
---

Git is one of the essential tools for a developer, the first thing anyone wanting to start a coding journey should learn, as emphasized in the [New Developer? You should’ve learned Git yesterday](https://codeburst.io/number-one-piece-of-advice-for-new-developers-ddd08abc8bfa) article from _Brandon Morelli_.

In this article, I will not talk about what `git` is, or why you should use it. Instead, I will show you how you can be more efficient using it thanks to aliases.

In the following sections, I go through all my aliases, why and how I use them. If you are in a hurry, you can go directly to the last section or to my [github config files repository](https://github.com/SimonDosda/config-files) to have the complete list of them.

## Basic Shortcuts

The first step of laziness. 
These aliases will shorten the number of characters you type. 
The one I use the most is `co` for `checkout`. 
I have defined other ones but don't use them that much, I prefer their variants which I will present later. 
Meanwhile, here is a list of classic aliases that I configured.

```bash
co = checkout
br = branch
ci = commit
st = status
```

More interestingly, you can also use this kind of shortcut to create new commands. The two ones I use a lot are for branch management.

```bash
nb = checkout -b  # create a new branch
del = branch -D  # delete a branch
```

These allow you to create a new branch and checkout on it by typing `git nb <branch-name>`, or to delete one by typing `git del <branch-name>`.

## Enhanced Shortcuts

I find some default behaviors of git quite annoying.

For instance, you need to set the name of the remote branch when you first push it, whereas I think I rarely come across the situation where I want this name to be different from the one of my local branch.

To avoid doing this every time, I use an alias, `git p`, that pushes the branch while setting the upstream branch to the same name.

```bash
# push to the same branch name
p = !git push -u origin $(git rev-parse --abbrev-ref HEAD)
```

When I commit, I usually want to stage all my modifications, and of course, add a message to my commit. I have set an alias allowing me to do this all at once: `git c “<my-commit-message>” `. 
If I want to commit a work in progress, I do `git wip`, which commits everything with `WIP` as a message.

```bash
# stage and commit all the changes
c = !git add --all && git commit -m
# stage and commit all the changes with WIP as a commit message
wip = !git add --all && git commit -m 'WIP'
```

Finally, I have a `git drop` alias to get rid of everything I did since the last commit. 

```bash
# drop current changes
drop = !git reset --hard HEAD  
```

## Status helpers

I have a couple of aliases to give me some information about the current state of my repository.

The first one is `git state`.
It fetches the remote repository (while removing deleted distant branches and fetching tags, some default behaviors of git I don’t understand), and then display local branches status using the verbose mode.

The other one is `git ll <n>`. 
It lists the n last commits of the current branch on one line.

```bash
# give a quick look at the state of the repo
state = !git fetch --prune && git fetch --tags && clear && git branch -vv && git status
# list the n last commits
ll = !git log --oneline -n
```

## Rebasing helpers

[My git workflow]({% post_url 2022-01-03-git-rebase-workflow %}) is based on a clean history, with one and only one commit for each feature or fix of the application (following [conventional commits](https://www.conventionalcommits.org/) specification), and no merge commits.

Unfortunately, while this gives a clean history, it also means using a lot of rebasing and having to force push on your repository.

To do so, I have the forced version of the push presented before, `git fp`. 
I also added an alias to amend everything in the current commit, `git amend`. 
The combination of those two is `git afp`, for **a**mend and **f**orce **p**ush, redoubtably efficient.

```bash
# force push to the same branch name
fp = !git push origin $(git rev-parse --abbrev-ref HEAD) -f
# amend everything in the current commit
amend = !git add --all && git commit --amend --no-edit
# amend and force push
afp = !git amend && git fp
```

To be more efficient in managing rebasing, I have set several aliases.

The first one, `git rom` (for **r**ebase **o**n **m**ain), rebases the current branch on the main branch of the remote repository (after fetching it).

Then there is `git rh` (for **r**eset **h**ard), which can be seen [as a force pull]({% post_url 2022-01-05-git-force-pull %}), and resets the current branch with the remote one.

Finaly, git ri <n> (for **r**ebase **i**nteractive) allows an interactive rebasing of the n last commits by typing `git ri <n>`.

```bash
# rebase on main origin branch                                                     
rom = !git fetch --all && git rebase $(git rev-parse --abbrev-ref origin/HEAD)
# reset the branch to the distant one
rh = !git fetch && git reset --hard origin/$(git rev-parse --abbrev-ref HEAD)
# interactive rebasing
ri = "!f() { git rebase -i HEAD~$1; }; f"
```

## But how to set these aliases?

You can set an alias with the config command of git, e.g., `git config --global alias.co checkout`, but the easiest is to write them directly in the `.gitconfig` file in your home directory, in the `[alias]` section.

Here is the list of all my aliases presented above (plus `git cp`, which copies the current commit hash, handy for cherry-picking).

```bash
co = checkout
br = branch
ci = commit
st = status

nb = checkout -b  # create a new branch
del = branch -D  # delete a branch

# give a quick look at the state of the repo
state = !git fetch --prune && git fetch --tags && clear && git branch -vv && git status
# list the n last commits
ll = !git log --oneline -n

# stage and commit all the changes
c = !git add --all && git commit -m
# stage and commit all the changes with WIP as a commit message
wip = !git add --all && git commit -m 'WIP'
# drop current changes
drop = !git reset --hard HEAD

# push to the same branch name
p = !git push -u origin $(git rev-parse --abbrev-ref HEAD)
# force push to the same branch name
fp = !git push origin $(git rev-parse --abbrev-ref HEAD) -f
# amend everything in the current commit
amend = !git add --all && git commit --amend --no-edit
# amend and force push
afp = !git amend && git fp

# rebase on main origin branch
rom = !git fetch --all && git rebase $(git rev-parse --abbrev-ref origin/HEAD)
# reset the branch to the distant one
rh = !git fetch && git reset --hard origin/$(git rev-parse --abbrev-ref HEAD)
# interactive rebasing
ri = "!f() { git rebase -i HEAD~$1; }; f"

# copy current hash in the clipboard
cp = !git rev-parse HEAD | xargs echo -n  | xclip -selection clipboard
```

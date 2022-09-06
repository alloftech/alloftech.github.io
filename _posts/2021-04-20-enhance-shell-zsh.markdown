---
layout: post
title: An Enhanced Shell With ZSH
date: 2021-04-20
cover_image: 2021-04-20-feature.jpg
author: Simon Dosda
categories: shell tool
excerpt_separator: <!--more-->
last_modified_at: 2021-07-07
---

When I started my first work as a developer, the rules were clear:

> We request our employees to work on Unix systems so they get used to our servers' environment.
>
> <cite>My New Boss</cite>

Which meant I had to switch to Linux.

<!--more-->

Not that I had never tried it before, but every time I couldn't see a real benefit to it, while hardware recognition was often a problem and it lacked some software like the Adobe suite.

So I jumped on the Linux bandwagon, and I loved it!

When it comes to coding, Linux is fantastic. You have great scripting functionalities and easy use of main developer tools like git, node, python, docker, etc.

All of this is significantly due to the shell! While most of humanity has forgotten about command-line interfaces to the benefit of more accessible graphical user interfaces, the shell remains an incredible tool for developers seeking automation and efficiency.

As one of our main tools, it would be too bad not to shape it to our needs. 

In this article, I will present my current setup. I am very interested in knowing yours, though; do not hesitate to share it in the comment section.

## Meet the Z shell

While the default _bash_ shell does a great job, there are several reasons to prefer its younger cousin, _zsh_.

Among those, it comes with a better auto-completion feature, combined with a spelling correction and approximate completion when a command is entered.

![zsh spelling correction](/assets/images/2021-04-20-autocomplete.png)
_zsh spelling correction in action_

It also provides convenient usage features such as [search patterns using globbing](https://linuxaria.com/howto/globbing-con-zsh) or the use of _vi_ when typing a command (you can enable it with `set -o vi`).

Last but not least, it provides a lot of customization of the prompt or functionalities with plugins, which I will come back to shortly.

## How to install it?

Before starting to customize our shell, we need to set _zsh_ as our default shell.

```bash
chsh -s /bin/zsh
```

The easiest way to customize our shell is to use a framework, the most well-known being [Oh My ZSH](https://ohmyz.sh/). You can install it with a simple command.

```bash
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

And that's it!

Now let's dive into the customization of our z shell.

## Some useful plugins

[Many plugins](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins) are already downloaded by _Oh My Zsh_. You can enable the one you want by adding them to the plugins list of your `.zshrc` file.

Here are the plugins I use on my end:

- [git](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git): provides a set of shell aliases for git like `g: git`, `gco: git checkout` or `gcb: git checkout -b`.

- [extract](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/extract): allows you to extract a file by typing `extract file.tar.gz` (can be used with the option `-r` to remove the original file). Much easier to remember than `tar zxvf file.tar.gz`!

- [virtualenv](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/virtualenv): displays the current python virtual environment name in the prompt (see below for its customization)

- [zsh-autosuggestion](https://github.com/zsh-users/zsh-autosuggestions): the one I can't live without anymore! It displays auto-completion based on the previous commands typed.

For `zsh-autosuggestion`, I use it with an extra configuration, `bindkey '^ ' forward-word`, that allows you to accept a suggestion word for word by typing `ctrl + space` (while hitting the right arrow will enter the complete line).

I also add the following line to the `.zshrc` file: `ZSH_AUTOSUGGEST_STRATEGY=(history completion)`. It allows the autosuggestion to fall back to the system autocompletion, the one you will get by hitting the tab key, if there is no history.

![zsh-autosuggestion](/assets/images/2021-04-20-suggest.png)
_zsh autosuggestion, once you have tried it, there is no coming back_

## A custom display

_Oh My Zsh_ comes with a lot of [predefined themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) for your prompt, and you might find the one you want there.

This is how I started, but I was quickly frustrated. Either I liked the colors of one but not the display, or I liked the display, but the colors were not to my taste.

Trying several themes can be quite time-consuming, so I decided to start from a theme I liked and customized it to get what suited me. In my case, this is how it looks like:

![my custom display](/assets/images/2021-04-20-display.png)

It is pretty straightforward. It displays (when relevant) the name of your _python_ environment (as _python_ is one of my primary programming languages), then the name of the computer, the local directory in relative format, and the current git branch (when relevant as well).

I like the simplicity and compactness: all the information that I need is here, on one line, and with no loss of space. 

This is very personal, and I strongly advise you to configure your theme to best suit your taste.

To do so, go to the theme file that you want to modify (in my case `~/.oh-my-zsh/themes/geoffgraside.zsh-theme`) and adapt it to your need.

Here is what I came up with:

```bash
PROMPT='%{$fg[red]%}$(virtualenv_prompt_info)%{$reset_color%}\
%{$fg[cyan]%}%n%{$reset_color%}:\
%{$fg[green]%}%~%{$reset_color%}\
$(git_prompt_info) %(!.#.$) '
ZSH_THEME_GIT_PROMPT_PREFIX=" %{$fg[yellow]%}("
ZSH_THEME_GIT_PROMPT_SUFFIX=")%{$reset_color%}"
```

Each value is framed with `%{$fg[<color>]%}<value>{$reset_color%}` to set the foreground color.

`$(virtualenv_prompt_info)` displays the _python_ virtual environment if relevant (thanks to the `virtualenv` plugin presented above), `%n` the computer's name, and `%~` the relative path.

For the git branch name, I use `$(git_prompt_info)` with a specific prefix and suffix that allows me to get rid of displaying `git:` before the branch name.

## Final Thoughts

I will stop here, that's already quite a perspective on _zsh_. However, there is much more to discover about it, and I hope I gave you a desire to take ownership of your shell.

The best thing I can advise you is to give it a try and then progressively add functionalities that suit you.

You can get the details of my `.zshrc` config file on my [github config files repository](https://github.com/SimonDosda/config-files).

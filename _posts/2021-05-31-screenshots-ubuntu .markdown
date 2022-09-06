---
layout: post
title: How To Take Static And Animated Screenshots On Ubuntu
date: 2021-05-31
cover_image: 2021-05-31-flameshot.gif
author: Simon Dosda
categories: tooling linux
last_modified_at: 2021-07-21
---

Picking tools to take screenshots is rarely seen as a primary concern, but I believe it is a considerably underrated topic. I do take a lot of them in my daily job: to document an issue, to show something to a member of my team, to illustrate a presentation, a blog article, and so on.

For a long time, I was just using the default software on Ubuntu, but I was frustrated that they were no way to annotate them to highlight a particular part. Moreover, I often felt the need to show things with an animated screenshot but did not know how to do so.

For something I do several times a day, spending some time to find the best fitting tools to my needs was a good investment, and I could not work anymore without the two following programs.

## _Flameshot_, to take customized screenshots

First on the list is [Flameshot](https://flameshot.org/), a powerful screenshot software that allows you to take a capture of your screen and annotate it easily.

Here is a quick demo where I use most of its features.

![flameshot in action](/assets/images/2021-05-31-flameshot.gif)
_Flameshot in action_

When you launch _Flameshot_, the first thing you have to do is to define your selection. You do it by just drawing a rectangle, but you can also hit enter if you want a full-screen capture.

Once your capture is done, you can annotate it. To do so, _Flameshot_ provides you with many features: pen, line, arrow, rectangle, circle, text, marker, and so on.

You are also able to undo and redo any action.

Once you are satisfied with the result, you can either save it or copy it in your clipboard to be able to paste it directly.

This tool is so convenient that I have set up a shortcut for it, so I just have to type `Ctrl+Super+P` to make it pop up.

To do so, add a custom shortcut in _Settings → Keyboard Shortcuts_ with the command `flameshot gui`. You can also add a default folder for the captures saving with the `-p` option like this: `flameshot gui -p ~/captures`.

![flameshot shortcut setting](/assets/images/2021-05-31-flameshot-shortcut.png)

## _Peek_, to record animated GIFs

While _Flameshot_ covers 90% of my need, there are times when I don't want to take a static screenshot but instead an animated capture of my screen.

I find this particularly useful when I want to show a feature or a bug that occurs after an action like a click. For instance, to show the features of _Flameshot_ as I did in the first part of this article!

To do so, my go-to guy is [Peek](https://github.com/phw/peek). _Peek_ is a straightforward tool that does one thing, recording animated GIFs of your screen, and does it well.

Here is a _Peek_ recording another _Peek_ recording my screen, a _Peek-ception_! 😅

![peek in action](/assets/images/2021-05-31-peek.gif)
_Peek in action_

As you can see, this software is very minimalist and efficient. When you open it, you define your screen capture area. Then, if needed, you can change the output file format. Finally, you can start recording.

Peek will then behave like it doesn't exist until you hit `stop` and save your capture.

I don't need animated captures as often as screenshots, but when I do, I am very happy to have this tool. As for _Flameshot_, I added a shortcut for _Peek_ to launch it by hitting `Ctrl+Alt+P`.

To do so, add another custom shortcut in _Settings → Keyboard Shortcuts_ with the command `peek`.

![peek shortcut setting](/assets/images/2021-05-31-peek-shortcut.png)

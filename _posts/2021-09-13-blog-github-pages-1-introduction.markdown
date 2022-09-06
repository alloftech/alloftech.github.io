---
layout: post
title: Build A Portfolio With A Blog Using GitHub Pages
date: 2021-09-13
last_modified_at: 2021-09-13
cover_image: 2021-09-13-feature.jpg
author: Simon Dosda
categories: ruby jekyll
---

This series of articles will show you how to quickly and freely deploy a personal portfolio website with a blog.

At the end of this tutorial, you will know how to build and host your own [Jekyll](https://jekyllrb.com/) blog on [GitHub Pages](https://pages.github.com/), how to create new pages or blog posts, and how to customize them.

We will also cover more advanced topics like adding a comment system directly in our code using [Staticman](https://staticman.net/) and integrating free privacy-friendly analytics using [Umami](https://umami.is/).

I divided this tutorial into several parts:

- Introduction **<- you are here**
- [Setting Up]({% post_url 2021-09-14-blog-github-pages-2-setup %})
- [Create Content]({% post_url 2021-09-15-blog-github-pages-3-content %})
- [Customize Display]({% post_url 2021-09-16-blog-github-pages-4-custom %})
- [Commenting System - Part 1]({% post_url 2021-09-17-blog-github-pages-5-comment-1 %})
- [Commenting System - Part 2]({% post_url 2021-09-18-blog-github-pages-6-comment-2 %})
- [Analytics]({% post_url 2021-09-19-blog-github-pages-7-analytics %})

## Foreword

When I started this blog, one piece of advice made a lot of sense for a developer wanting to start a blog: Even though you are a developer, don’t lose your time building your blog, focus on writing content and publish it on any blogging platform.

I am totally in agreement with this advice. It is too easy for us to spend a lot of time creating a blog from scratch, missing the main point: a blog is about what you have to say. Spending your time to develop your blog is just disguised procrastination.

But the “create your own blog” team had a point though: creating your blog allows you to own your content.

I eventually went for a trade-off between these two approaches: setting my own blog very quickly using GitHub Pages while republishing my content on [dev.to](https://dev.to/simondosda), which allows me to reach a broader audience.

After deploying my blog on _GitHub Pages_, I realized it was also a perfect solution to host a portfolio. GitHub is a natural place to showcase your work as a developer, and this will be a big plus for recruiters or clients if you are a freelancer.

So even if the blogging part does not appeal to you, you still might be interested in following this tutorial.

## Why Github Pages?

There are many solutions to create your blog or portfolio, and I am far from trying them all.
Using Github Pages is not the perfect solution, but it has some noticeable pros for developers.

First, you are probably already quite familiar with the use of _Git_ and _GitHub_. The fact that all your content will be in a Git folder is lovely and provides versioning for your articles and pages.

It also makes sense to have a website link to your _GitHub_ account where you centralize all your development-related content: blog articles, portfolio, side projects, and so on. You can see this as your personal showcase.

And last but not least, deploying and hosting your website on _GitHub Pages_ is really easy and totally free!

## How will it work?

In this tutorial, we will use a static site generator named [Jekyll](https://jekyllrb.com/). _Jekyll_ is the framework used by _GitHub_ to power _GitHub Pages_, which gives us the significant advantage of having _GitHub_ building our pages without needing to do anything.

_Jekyll_ builds HTML pages from markdown documents, which is excellent as this is the standard used by many blogging platforms and tools. Once you add an article in a markdown format, pushing it to your deployment branch will update your website.

## What are the cons?

There is no perfect solution to build a website, and it is essential to know the limitation of your stack when choosing it.

As a static site generator, Jekyll will build all pages of our website at deployment. This is nice as it provides a fast navigation experience, compared to a website using a back-end for which pages would be build when the user navigates the website.

But it also means that our pages will not be dynamic, with no interactivity with a database. Of course, you can still add some interactivity using javascript and AJAX calls to a service, but it means you will need a separated back-end for this.

Hosting a _Jekyll_ website on _GitHub Pages_ is probably the easiest way, but it also comes with a significant drawback: the limited list of plugins that you can use. You can find the (small) list of available plugins [here](https://pages.github.com/versions/).

## Ready to start?

Now that we have covered what we will achieve in this tutorial, it is time to start the work and [set up our project]({% post_url 2021-09-14-blog-github-pages-2-setup %}).
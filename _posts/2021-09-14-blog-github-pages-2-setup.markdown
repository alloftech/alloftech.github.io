---
layout: post
title: Setup Your Free Portfolio With A Blog Using GitHub Pages
date: 2021-09-14
last_modified_at: 2021-09-14
cover_image: 2021-09-14-feature.jpg
author: Simon Dosda
categories: ruby jekyll
---

This article is part of a series showing you how to quickly and freely build and host your own [Jekyll](https://jekyllrb.com/) blog on [GitHub Pages](https://pages.github.com/). This series will also cover more advanced topics like adding a comment system directly in our code using [Staticman](https://staticman.net/) and adding privacy-friendly but still free analytics using [Umami](https://umami.is/).

I divided the tutorial into several parts:

- [Introduction]({% post_url 2021-09-13-blog-github-pages-1-introduction %})
- Setting Up **<- you are here**
- [Create Content]({% post_url 2021-09-15-blog-github-pages-3-content %})
- [Customize Display]({% post_url 2021-09-16-blog-github-pages-4-custom %})
- [Commenting System - Part 1]({% post_url 2021-09-17-blog-github-pages-5-comment-1 %})
- [Commenting System - Part 2]({% post_url 2021-09-18-blog-github-pages-6-comment-2 %})
- [Analytics]({% post_url 2021-09-19-blog-github-pages-7-analytics %})

Now, let's see how we can set up and deploy our website.

## Create your GitHub Pages repository

To deploy your website using _GitHub Pages_, you need to create a new public repository using the following convention for the name: `<username>.github.io`.

![GitHub Pages Repository](/assets/images/2021-09-14-repo.png)

Once you have done this, the content of your repository will be deployed on `https://<username>.github.io`. You can try adding an `index.html` file at the root of your repository with "Hello World" or anything you fancy written in it to check everything works as expected.

Note that you can deploy a website from any repository, but in this case, the associated URL will be `https://<username>.github.io/<repository-name>`. This feature is very convenient to deploy documentation of your projects, for instance.

For this tutorial, I will use a specific repository called [gp-blog](https://github.com/SimonDosda/gp-blog) (for Github Pages Blog), which will therefore deploy at [https://simondosda.github.io/gp-blog](https://simondosda.github.io/gp-blog), but you most probably want to deploy your portfolio at the root folder.

Now that our git repository is ready, let's see how to set up _Jekyll_ in it.

## Create your Jekyll static website

Jekyll is a Ruby Gem, so make sure you have all the prerequisites installed to use it. You can find their list and the procedure to install them on your OS at [https://jekyllrb.com/docs/installation/](https://jekyllrb.com/docs/installation/).

Once you have done this, install _Jekyll_ and _Bundler_ gems.

```bash
gem install jekyll bundler
```

We can now initialize our _Jekyll_ project in the repository we just created with the following command.

```bash
jekyll new .
```

Then, open the _Gemfile_ file and follow the instruction to deploy on _GitHub Pages_ by commenting the `gem "jekyll"` line and uncommenting the `gem "github-pages"` one.

```bash
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
# gem "jekyll", "~> 4.2.0"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages", "~> 214", group: :jekyll_plugins
```

For the version (here 214), you can find the latest version [here](https://pages.github.com/versions/).

You can then type the command `bundle update` to install your dependencies and serve your website locally with the command `bundle exec jekyll serve --livereload`.

Your new blog is now served on [localhost:4000](http://localhost:4000). As you can see, it needs some customization.

## Edit the global configuration

We can now start customizing our new blog. The first thing to do is to update the `_config.yml` file. There you can edit your blog title, its description, your email address, and _GitHub_ username.

You can also enter your _Twitter_ username or comment the corresponding line if you don't have any.

You can also add the property `show_excerpts: true` to display posts' excerpts on the home page.

More anecdotical, you can change the permalink generation for pages with the `permalink` attribute. I like to use a less nested structure like the following.

```yaml
permalink: /:collection/:year-:month-:day-:title:output_ext
```

Even though we use the `--livereload` option, it will not consider changes done in the `_config.yml` file. You need to kill your server and relaunch it, and you should see your changes appear in your browser.

If everything is ok, you can commit your changes and push them.

Note that you can edit the branch which deploys your website on your GitHub repository in _Settings → Pages → Source_.

Here we are. We now have our blog backbone up and running!

You can find the code for this part [here](https://github.com/SimonDosda/gp-blog/tree/step-1-setup).

Our next step is now to [add some content to our website]({% post_url 2021-09-15-blog-github-pages-3-content %}).
---
layout: post
title: Customize Your Jekyll Website
date: 2021-09-16
last_modified_at: 2021-09-16
cover_image: 2021-09-16-feature.jpg
author: Simon Dosda
categories: ruby jekyll
---

This article is part of a series showing you how to quickly and freely build and host your own [Jekyll](https://jekyllrb.com/) blog on [GitHub Pages](https://pages.github.com/). This series will also cover more advanced topics like adding a comment system directly in our code using [Staticman](https://staticman.net/) and adding privacy-friendly but still free analytics using [Umami](https://umami.is/).

I divided the tutorial into several parts:

- [Introduction]({% post_url 2021-09-13-blog-github-pages-1-introduction %})
- [Setting Up]({% post_url 2021-09-14-blog-github-pages-2-setup %})
- [Create Content]({% post_url 2021-09-15-blog-github-pages-3-content %})
- Customize Display **<- you are here**
- [Commenting System - Part 1]({% post_url 2021-09-17-blog-github-pages-5-comment-1 %})
- [Commenting System - Part 2]({% post_url 2021-09-18-blog-github-pages-6-comment-2 %})
- [Analytics]({% post_url 2021-09-19-blog-github-pages-7-analytics %})

Now that we have started adding content to our website, let's see how we can customize its appearance.

## How Jekyll generates pages

One of the reasons I like _Jekyll_ is that it provides you a very clean directory as it hides most of the magic behind the website creation.
However, the drawback is that it is easy to feel lost and not to know how to update your theme when it comes to customizing it.

Indeed, layouts are defined by your theme and are not present in your local folder.

So before doing anything, we need to look at our [minima theme repository](https://github.com/jekyll/minima). Please make sure you look at the branch of your current version.
In my case, it is the 2.5 one that is installed (you can have this info in the `Gemfile` file), so I am looking at the [2.5-stable branch](https://github.com/jekyll/minima/tree/2.5-stable).

Our blog structure mainly relies on two folders:

- `_layouts`: contains the layouts which are used in the Front Matter metadata of each markdown file.
- `_includes`: contains snippets of HTML code that are used in layouts.

For instance, if we look at the code of the `post` layout, we see the following (shortened for comprehension).

{% raw %}

```html
<!-- _layouts/post.html -->
---
layout: default
---
<article
  class="post h-entry"
  itemscope
  itemtype="http://schema.org/BlogPosting"
>
  <header class="post-header">
    <h1 class="post-title p-name" itemprop="name headline">
      {{ page.title | escape }}
    </h1>
    ...
  </header>

  <div class="post-content e-content" itemprop="articleBody">{{ content }}</div>
  ...
</article>
```

There is a lot to comment on here.

The first thing we see is that the blog layout uses the `default` layout.
The way layout nesting works is that everything that you have below your Front Matter will be injected in the `{{ content }}` variable of its parent layout.

For instance when you write a blog post using this layout, its content is injected in the `<div class="post-content e-content" itemprop="articleBody">{{ content }}</div>` block.

We can also see the use of Front Matter metadata with the title of the post.
The title is displayed using the `page` variable, which contains all the variables defined in the Front Matter of a template.

These variables can be overridden by the children of the template. In this case, there is no title defined in the Front Matter of our template, it only makes sense for posts using this template.

Now, if we look at the default layout, this is what we see.

```html
<!-- _layouts/default.html -->
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: 'en' }}">
  {% include head.html %}

  <body>
    {% include header.html %}

    <main class="page-content" aria-label="Content">
      <div class="wrapper">{{ content }}</div>
    </main>

    {% include footer.html %}
  </body>
</html>
```

The default layout represents the base structure of all our pages.
Besides the `content` variable where the child layout is injected, it uses several `include` to inject HTML code.

Let's now dive into them and start customizing our blog by adding a favicon.

## Adding a favicon

Let's start gently by adding a favicon to our website. If you are familiar with HTML, you know that the favicon is defined in the `head` tag. For this theme, it is the `_includes/head.html` file that contains this tag.

You can override any file of your theme by putting it in your own project.
In this case, we can create our own `_includes/head.html`, copy the code from GitHub and modify it to add our favicon.
{% endraw %}

For my favicon, I generated one using [a lion emoji](https://favicon.io/emoji-favicons/lion) for the sake of the exercise.

{% raw %}
```html
<!-- _includes/head.html -->
<head>
  <meta charset="utf-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  {% seo %}
  <link rel="stylesheet" href="{{ '/assets/main.css' | relative_url }}" />
  {% feed_meta %} 
  {% if jekyll.environment == 'production' and site.google_analytics %} 
    {% include google-analytics.html %} 
  {% endif %}

  <link
    rel="apple-touch-icon"
    sizes="180x180"
    href="{{ '/apple-touch-icon.png' | relative_url }}"
  />
  <link
    rel="icon"
    type="image/png"
    sizes="32x32"
    href="{{ '/favicon-32x32.png' | relative_url }}"
  />
  <link
    rel="icon"
    type="image/png"
    sizes="16x16"
    href="{{ '/favicon-16x16.png' | relative_url }}"
  />
</head>
```

## Modifying the header

The same way we overrode our `head.html` fille, we can override any file to customize your website as we want.

For instance, let's modify our header by adding our new logo.

To do so we need to add and modify the `_includes/header.html` file.

```html
<!-- _includes/header.html --> 
<header class="site-header" role="banner">
  <div class="wrapper">
    {% assign default_paths = site.pages | map: "path" %} 
    {% assign page_paths = site.header_pages | default: default_paths %}
    <a class="site-title" rel="author" href="{{ '/' | relative_url }}">
      <img src="{{ 'favicon-32x32.png' | relative_url }}" />
      {{ site.title | escape }}
    </a>
    ...
  </div>
</header>
```

## Customizing our CSS

There are several ways to modify our CSS. As you can see in the `head.html` file, CSS is currently pulled from `assets/main.css`.
In the development file, it is actually a `scss` file that is built from the `_sass` directory.

An option to customize our css is to copy/paste the `_sass` directory and edit its files, the same we did for the head file.

Another possibility is to generate a new file that will be loaded after the theme css, so that the rules we put in it will override the default ones. This is the solution I will show you now.

First, let's create the `assets/main.scss` file with the following code.

```scss
// assets/main.scss
---
---

@import "minima";
@import "custom";
```

Compared to the default file, I just added an import of the file `custom`. We can now create this file in the `_sass` folder and add some css.

```scss
// _sass/custom.scss
.site-title {
  color: orangered;
  &:visited {
    color: orangered;
  }
}
```

Here I modify the site title color to match our lion. My main point is to show you how you can do it, it is up to you to decide how you want to style your website.

## Adding a featured image to our posts

Something you will see in almost every blog is a featured image displayed in the blog list and at the top of an article.

Unfortunately, this is not currently managed by _Jekyll_, so let's implement this feature ourselves.

This time we want to modify the blog layout directly. We can create our own version in `_layouts/post.html`.

What we are going to do is to check for a `featured_image` variable, and if it exists, we will display it on top of our title by adding the following snippet.

```html
{% if page.featured_image %}
<div class="featured-image">
  <img src="{{ '/assets/' | append: page.featured_image | relative_url }}" />
</div>
{% endif %}
```

Let's add a featured image to our last post. Put the image you want in your assets folder and add its name in the post metadata.

```yaml
---
layout: post
title: "Write a Post"
date: 2021-06-31
categories: jekyll blogging
featured_image: featured-image.jpg
---
```

We can then add some CSS to our `custom.scss` file to style it.

```scss
// _sass/custom.scss
... 
.featured-image {
  margin-bottom: 50px;

  img {
    width: 100%;
    max-height: 250px;
    object-fit: cover;
  }
}
```

Here is the result.

![Post With Image](/assets/images/2021-09-16-post.png)

## Updating the home to display the featured image

Let's improve our home page.

First, copy the `home.html` in the `_layout` folder. Following the same principle as for the post layout, we can add our featured images.

```html
<!-- _layout/home.html -->
--- 
layout: default 
---

<div class="home">
  {% if page.title %}
    <h1 class="page-heading">{{ page.title }}</h1>
  {% endif %} 
  {{ content }} 
  {% if site.posts.size > 0 %}
    <h2 class="post-list-heading">
      {{ page.list_title | default: "Posts" }}
    </h2>
    <ul class="post-list">
      {% for post in site.posts %}
      <li>
        <div>
          {% assign date_format = site.minima.date_format 
            | default: "%b %-d, %Y" %}
          <span class="post-meta">
            {{ post.date | date: date_format }}
          </span>
          <h3>
            <a class="post-link" href="{{ post.url | relative_url }}">
              {{ post.title | escape }}
            </a>
          </h3>
          {% if site.show_excerpts %} 
            {{ post.excerpt }} {% endif %}
        </div>
        {% if post.featured_image %}
        <div class="featured-image">
          <img
            src="{{ '/assets/' | append: post.featured_image | relative_url }}"
          />
        </div>
        {% endif %}
      </li>
      {% endfor %}
    </ul>

    <p class="rss-subscribe">
      subscribe <a href="{{ '/feed.xml' | relative_url }}">via RSS</a>
    </p>
  {% endif %}
</div>
```

And update our `custom.scss` file.

```scss
// _sass/custom.scss
... .post-list > li {
  display: flex;
  flex-wrap: wrap-reverse;

  div:first-child {
    flex: 4 0 200px;
  }
  .featured-image {
    flex: 1 0 200px;
    margin-bottom: 0;
  }
}
```

{% endraw %}
And here is the result.

![Responsive Posts Image](/assets/images/2021-09-16-home.gif)

Here you are, you now have all the basics to create your personal blog at your own image.

You can find the code for this part [here](https://github.com/SimonDosda/gp-blog/tree/step-3-custom).

Our next step is now [to add a commenting system to our blog]({% post_url 2021-09-17-blog-github-pages-5-comment-1 %}).

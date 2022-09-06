---
layout: post
title: Follow Your Website Analytics With Umami
date: 2021-09-19
last_modified_at: 2021-09-19
cover_image: 2021-09-19-feature.jpg
author: Simon Dosda
categories: ruby jekyll
---

This article is part of a series showing you how to quickly and freely build and host your own [Jekyll](https://jekyllrb.com/) blog on [GitHub Pages](https://pages.github.com/). This series will also cover more advanced topics like adding a comment system directly in our code using [Staticman](https://staticman.net/) and adding privacy-friendly but still free analytics using [Umami](https://umami.is/).

I divided the tutorial into several parts:

- [Introduction]({% post_url 2021-09-13-blog-github-pages-1-introduction %})
- [Setting Up]({% post_url 2021-09-14-blog-github-pages-2-setup %})
- [Create Content]({% post_url 2021-09-15-blog-github-pages-3-content %})
- [Customize Display]({% post_url 2021-09-16-blog-github-pages-4-custom %})
- [Commenting System Part 1]({% post_url 2021-09-17-blog-github-pages-5-comment-1 %}) 
- [Commenting System - Part 2]({% post_url 2021-09-18-blog-github-pages-6-comment-2 %})
- Analytics **<- you are here**

Now that our blog is fully functional, our main job is to write interesting articles. 

But you are writing a blog and sharing it with the world, you probably want people to read it.
To do so you will have to bring traffic to your website, and will therefore need to be able to follow your it, know which article attracts the most people, on so on.


## Picking a Traffic Analytics solution

When it comes to gathering traffic data from your website, you mainly have two choices of products.

The first one is to use a free solution. 
Let's be honest, we are talking about [Google Analytics](https://analytics.google.com/analytics/web/) here, which is already pre-integrated with Jekyll.
It is indeed very easy to set up but raises some confidentiality issues as its trackers are very invasive, and all the data collected eventually goes to google.

If you are concerned about collecting your user data through a company that will use them, you can go for a more privacy-focused solution, like [Gauges](https://get.gaug.es/) or [Simple Analytics](https://simpleanalytics.com/). 
These companies will also allow you to collect your visitors' data, but without exploiting them. 
Most of the time, they are less privacy-invading and only collect relevant information. 
They also usually don't use cookies, allowing you to avoid having to put an annoying cookie banner for European visitors to comply with GDPR policies.

The counterpart for using a solution not exploiting your visitor's data? 
Well, companies need to make profits to live, so you need to pay for it. 
This is totally ok and understandable, but you might not want to spend money just to know how many visitors come to your blog, which is already a generous donation of your time to the community.

Fortunately, development is an amazing world and some analytics companies have open-sourced their code. 
Here, we will see how we can deploy one of them, [Umami](https://umami.is/), and how to use it for our website.  

## Deploying Umami on Heroku

As we already did for our comment system, we will deploy _Umami_ on _Heroku_. You can see how to do so [here](https://umami.is/docs/running-on-heroku), but we will do it together anyway.

First, you need to fork the [Umamy repository](https://github.com/mikecao/umami) on _GitHub_.

Then, go to your _Heroku_ account, create a new app, and under the `Deploy` section choose `connect to Github` as `Deployment method`, search for the repository you just forked, and connect to it.

![Connect to Github](/assets/images/2021-09-19-connect-github.png)

We then need to add a database. Go to the `Ressources` tab, search for `Heroku Postgres` in the `Add-ons` section and install it.

![Add Heroku Postgres](/assets/images/2021-09-19-heroku-postgres.png)

Now we need to create the required tables. 
Click on your new add-on (open in a new tab), then go to `Settings -> View Credentials`. 
You will get everything you need to connect to your database. Personally, I have installed the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) so I just run the corresponding line in my terminal. 

![Connection to Postgres](/assets/images/2021-09-19-postgres-connection.png)

I then copied the lines from the [schema.postgresql.sql](https://github.com/SimonDosda/umami/blob/master/sql/schema.postgresql.sql) file to init the tables. 
You can check everything went ok by running the `\dt` command.

```sql
\dt

List of relations
 Schema |   Name   | Type  |     Owner      
--------+----------+-------+----------------
 public | account  | table | ivcqpajmowwzfs
 public | event    | table | ivcqpajmowwzfs
 public | pageview | table | ivcqpajmowwzfs
 public | session  | table | ivcqpajmowwzfs
 public | website  | table | ivcqpajmowwzfs
(5 rows)
```

Then go back to the app page and, in the `Settings -> Config Vars`, add an `HASH_SALT` environment variable with any random string as a key. 

![App Environment Variables](/assets/images/2021-09-19-config-vars.png)

Phew, we finally ended the configuration of our app!

You can now go to the `Deploy` section and click `Deploy Branch` in the `Manual deploy` part.

![Deploy App](/assets/images/2021-09-19-deploy.png)

Once the build is over, you will be able to open the app and log into it with the username `admin` and the password `umami`.

![Umami Login](/assets/images/2021-09-19-umami-login.png)

The first thing you want to do is to change this password in `Settings -> Profile -> Change password`.

We now have to add our website to the application. To do so, go to `Settings -> Website -> Add website`

![Add Website](/assets/images/2021-09-19-add-website.png)

Click then on `get tracking code` to get the script line to add to your website head.

![Add Traking Code](/assets/images/2021-09-19-umami-traking-code.gif)

{% raw %}
```html
<!-- _includes/head.html -->
...
{%- if jekyll.environment == 'production' -%}
        <script async defer 
            data-website-id="<your-id>" 
            src="<your-src>">
        </script>
{%- endif -%}
```
{% endraw %}

Redeploy your website with it and visit it, then go to your Umami dashboard, you should see your first visit! 

![Analytics Display](/assets/images/2021-09-19-analytics.png)

Yeah, my first visitor, myself!

## Wrap up

Congrats, you have reached the end of this tutorial!

I hope it was beneficial to you and that you enjoyed it. You can find the code of this sample project [here](<https://github.com/SimonDosda/gp-blog>).

We covered quite a lot, from setting up and deploying your blog to its customization and the add of features like a commenting system or analytics.

A lot can still be done, and I encourage you to explore new possibilities from here.

If there is a subject you are particularly interested in, please share it in the comment section.

But in any case, don't forget that the most important on the website is the content ;).

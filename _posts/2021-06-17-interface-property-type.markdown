---
layout: post
title: Getting The Type Of An Interface Property In Typescript
date: 2021-06-17
cover_image: 2021-06-17-feature.png
author: Simon Dosda
categories: typescript trick
excerpt_separator: <!--more-->
last_modified_at: 2021-07-21
---

## Problem

The problem is quite simple, but its solution is not obvious.

Imagine that we are working on a _Typescript_ project, and we have defined an interface with several fields using different types.
Let's take as an example a simple interface representing a blog post.

<!--more-->

```typescript
interface BlogPost {
  title: string;
  content: string;
  publishDate: Date | null;
  isDraft: boolean;
}
```

Let's say now that we would like to provide a simple setter that will allow us to update a blog post while running some custom logic. For instance, let's consider that our setter will allow updating a post only if it is a draft.

In _Typescript_, we would write something like this.

```typescript
function updatePost(post: BlogPost, property: keyof BlogPost, value): boolean {
  if (!post.isDraft) {
    return false;
  }
  post[property] = value; // error: Type 'any' is not assignable to type 'never'.
  return true;
}
```

While the types of `post` and `property` are easy to define, the one for value is a bit more tricky, as it will depend on the property to update.

Currently, _Typescript_ considers it as `any` as we don't provide any type hint, which gives us the error `Type 'any' is not assignable to type 'never'.` when we try to update our property. But even if we put the error aside, we are losing the type checking capability of _Typescript_ here.

What we would like to do is to set the type of `value` to the type of the corresponding `property` from `BlogPost`. Let's see now how we can implement that.

## Solution

In _Typescript_, we can access the value of the property of an interface using brackets.

For instance, the following code works perfectly.

```typescript
function updateDate(post: BlogPost, value: BlogPost["publishDate"]): boolean {
  if (!post.isDraft) {
    return false;
  }
  post.publishDate = value;
  return true;
}

updateDate(post, 42);
// error: Argument of type '42' is not assignable to parameter of type 'Date | null'.

updateDate(post, new Date());
```

In our case, we want a generic function that will work with any given property of our interface.

To do so, we need to use a type variable for the property.

```typescript
function updatePost<BlogPostKey extends keyof BlogPost>(
  post: BlogPost,
  property: BlogPostKey,
  value: BlogPost[BlogPostKey]
): boolean {
  if (!post.isDraft) {
    return false;
  }
  post[property] = value;
  return true;
}

const post: BlogPost = {
  title: "my post",
  content: "my awesome content",
  publishDate: null,
  isDraft: true,
};

updatePost(post, "publishedDate", null);
// error: Argument of type '"publishedDate"' is not assignable to parameter of type 'keyof BlogPost'.

updatePost(post, "publishDate", 42);
// error: Argument of type '42' is not assignable to parameter of type 'Date | null'.

updatePost(post, "publishDate", new Date());
// ok
```

Adding the type variable `BlogPostKey` for the property type allows us to use it for the `value` type while still ensuring that the property is a key of `BlogPost` using `extends keyof BlogPost`.

I hope that trick can be helpful to you!

<!-- adsense -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1865928774299375"
     crossorigin="anonymous"></script>

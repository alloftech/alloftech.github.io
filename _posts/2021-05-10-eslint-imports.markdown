---
layout: post
title: Automatically Remove Unused Imports From Your JS Projects
date: 2021-05-10
last_modified_at: 2021-07-06
cover_image: 2021-05-10-feature.jpg
author: Simon Dosda
categories: javascript tool
excerpt_separator: <!--more-->
---

> Photo by [The Creative Exchange](https://unsplash.com/@thecreative_exchange).

Recently, I came across a substantial Angular project with a lot of unused imports. It is not a big deal, but it looked pretty messy, which I find to be a pity as automatically removing them is pretty straightforward.

<!--more-->

In this article, I will show how to do so for any node-based project using ESLint. It might sound like a very cosmetic thing, and it kind of is, but I believe having too many unused imports can hurt code readability.

And as a bonus, we will also sort our imports in alphabetical order.

## Add _ESLint_ to your project

[ESLint](https://eslint.org/) is a static code analyzer and will prevent you from making many dummy mistakes, like using undeclared variables or expecting an output from a function that doesn't have any.

It can also enforce code style rules, like the type of quotes you want to use or define if code lines should always end with semicolons, even though you will most likely use a code formatter like [Prettier](https://prettier.io/) to take care of this.

If you haven't used it yet, you will need to add *ESLint* to your project. You can easily install it and generate its configuration file with *npm*.

```bash
npm install eslint --save-dev
npx eslint --init
```

You can then check the errors and warnings from _ESLint_ by running it in your project.

```bash
npx eslint <source-directory>
```

## Automatically remove unused imports

To automatically remove unused imports, we will need to add the [eslint-plugin-unused-imports](https://www.npmjs.com/package/eslint-plugin-unused-imports) plugin.

Install it using npm:

```bash
npm install eslint-plugin-unused-imports --save-dev
```

Then add it to your configuration file; here with the recommended rules from the author:

```json
{
  "plugins": ["unused-imports"],
  "rules": {
    "no-unused-vars": "off",
    "unused-imports/no-unused-imports": "error",
    "unused-imports/no-unused-vars": [
      "warn",
      {
        "vars": "all",
        "varsIgnorePattern": "^_",
        "args": "after-used",
        "argsIgnorePattern": "^_"
      }
    ]
  }
}
```

Now, when you run *ESLint*, you should see error lines saying `error '<imported-var>' is defined but never used unused-imports/no-unused-imports` for the files where you have unused imports. Moreover, the last line should print the following line `X errors and Y warnings potentially fixable with the --fix option.`.

The number of errors should be superior to 0 unless you don't have any unused imports in your project. If that's the case, add some for the sake of this exercise ;).

Next, run `npx eslint <project-directory> --fix` and...voilà!

There should not be any unused import in your code anymore.

## Bonus: sort your imports by alphabetical order

Sorting imports by alphabetical order is the last thing I want to take care of. I don't think it really matters, even though it can be part of a company or a team's rules to do so.

In any case, *ESLint* allows us to do this automatically, so why deprive ourselves of it?

To benefit from this feature, you need to add the [sort-import](https://eslint.org/docs/rules/sort-imports) rule to your *ESLint* configuration file.

```json
{
  "rules": {
    ...
    "sort-imports": [
      "error",
      {
        "ignoreDeclarationSort": true
      }
  ]
}
```

Unfortunately, the `--fix` option will not automatically fix multiple lines errors. For this reason, I prefer to set `ignoreDeclarationSort` to `true`.

It is for the best anyway because this rule provides minimal customization to order your imports. And I don't think alphabetical order at line level makes sense without considering the kind of import; you don't want your local imports mixed with third-party libraries, for instance. If you are using `TSLint` though, check [ordered-imports](https://palantir.github.io/tslint/rules/ordered-imports/) that allows you to define your import order and fix multiple lines imports.

Now, running *ESLint* with the `--fix` option will reorder your multiple members' imports. For instance, `import { d, a, c, b } from e;` will be changed to `import { a, b, c, d } from e;`.

It doesn't hurt!

<!-- adsense -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1865928774299375"
     crossorigin="anonymous"></script>

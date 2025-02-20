---
layout: post
title:  "Nodejs project template"
date:   2024-02-10 18:58:00 +0200
categories: coding typescript
excerpt: "Simply run a new nodejs project using a simple project template skeleton as a starter."
---
I needed to simply run a new `nodejs` project and I wanted a **simple project template skeleton** as a starter.

After trying out a few options, here is what worked for me out of the box:

[Nodejs starter template][nodejs-starter-template]{:target="_blank" rel="noopener"}


If you are having trouble setting up a linter for your project, please check this setup:

[Nodejs lint setup][nodejs-lint-setup] 

{% highlight javascript %}
// eslint-disable-next-line no-undef
module.exports = {
  root: true,
  env: {
    browser: true,
    es2020: true
  },
  extends: 'eslint:recommended',
  parserOptions: {
    ecmaVersion: 12,
    sourceType: 'module'
  },
  rules: {
    semi: [
      2,
      'always'
    ]
  }
};
{% endhighlight %}

[nodejs-starter-template]: https://github.com/bahricanyesil/nodejs-starter-template

[nodejs-lint-setup]: https://github.com/MichaelCurrin/node-project-template/blob/master/.eslintrc.js

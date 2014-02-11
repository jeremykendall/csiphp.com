---
layout: post
title: "No Way That's Real"
date: 2013-08-27 21:25
comments: true
categories: [Trolling, PHP]
---

I thought that Graham was the most [devious PHP code troll I'd ever met](http://csiphp.com/blog/2013/08/19/artisan-level-code-trolling/).
Turns out I was wrong.  Dead wrong.

This tweet:

{% tweet https://twitter.com/sabiddle/status/372532408274268160 %}

led me to this post: ["Creating a user from the web problem"](http://www.reddit.com/r/PHP/comments/1l7baq/creating_a_user_from_the_web_problem/),
which includes this code:

``` php
shell_exec("sudo useradd -p $encpass -g groupname -s /bin/bash $username");
```

I honestly don't think this post is for real.  I think it's a brilliant, amazing,
downright *epic* troll.  Kudos to you, sir or ma'am.  Kudos.

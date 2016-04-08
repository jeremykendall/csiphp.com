---
author: jeremykendall
comments: true
date: 2012-01-11 07:30:43
layout: post
slug: who-needs-regular-expressions
title: Who needs regular expressions?
wordpress_id: 487
categories:
- PHP
author: "Jeremy Kendall"
---

Thanks to [Moses Ngone](http://twitter.com/#!/mosesngone) for submitting this super sweet router snippet.

Moses writes:

> "If the regular expression route gets tough, be more specific."

Indeed.

```php
<?php

$vars = str_replace(".php", "", $_SERVER['REQUEST_URI']);
$vars = str_replace(".html", "", $vars);
$vars = str_replace("pics/",'',$vars);
$vars = str_replace("/pics",'',$vars);
$vars = str_replace("pix/",'',$vars);
$vars = str_replace("/pix",'',$vars);
$vars = str_replace("photos/",'',$vars);
$vars = str_replace("/photos",'',$vars);
$vars = str_replace("photo/",'',$vars);
$vars = str_replace("/photo",'',$vars);
$vars = str_replace("gallery/",'',$vars);
$vars = str_replace("/gallery",'',$vars);
$vars = str_replace("galleries/",'',$vars);
$vars = str_replace("/galleries",'',$vars);
$vars = str_replace("//","/",$vars);
$var_array = explode("/",$vars);
```

Pro tip: If you find yourself doing something like this, don't.  There are [tons](http://www.slimframework.com/) of [excellent](http://symfony.com/) PHP [frameworks](http://zendframework.com/) that take care of [routing](https://github.com/auraphp/Aura.Router) for you.  If you really want to write your own, spend some time reading some framework's source code and unit tests to see how a few other folks have done it.

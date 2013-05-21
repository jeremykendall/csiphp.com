---
author: jeremykendall
comments: true
date: 2011-09-16 10:15:26
layout: post
slug: god-help-the-dev-i-catch-doing-this
title: God help the dev I catch doing this
wordpress_id: 315
categories:
- PHP
---

1. Make decision to prematurely optimize application
2. Choose the least helpful optimization first
3. Append the static asset text files with .php (AJS.jsmin.js.php in this case)
4. Use the [`header()`](http://us.php.net/manual/en/function.header.php) function to [manage caching](http://condor.depaul.edu/dmumaugh/readings/handouts/SE435/HTTP/node24.html) of static assets
5. Include new awesome cache controlled files in application
6. Leave it to maintenance programmer to resolve all the "Cannot modify header information" warnings

```php
<?php
// let the user cache this for to speed up their experience -- 10/23/2008
header("Content-type: text/javascript; charset: UTF-8");
header("Cache-Control: must-revalidate");
$offset = 60 * 60 * 24 * 90;90// 90 days for starters
$ExpStr = "Expires: " . 
gmdate("D, d M Y H:i:s",
time() + $offset) . " GMT";
header($ExpStr);
?>

AJS={BASE_URL:"",drag_obj:null,drag_elm:null,_drop_zones:[], . . .
```

(NOTE: The ellipsis above indicates snipped, minified javascript)

Along with promising to visit death upon developers using a technique like this, I'll pass along at least one better idea.  Here's an alternative from Greg Aker in his post, "[Bust 'yo cache!](http://www.gregaker.net/2011/aug/31/bust_yo_cache/)".  Add yours in the comments

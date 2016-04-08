---
author: jeremykendall
comments: true
date: 2012-09-21 08:00:04
layout: post
slug: what-idiot-wrote-this-oh-wait-it-was-me
title: What idiot wrote this? Oh, wait, it was me.
wordpress_id: 615
categories:
- "mea culpa"
- PHP
author: "Jeremy Kendall"
---

Starting with this post, we have a new category here at CSI: PHP.  It's called 
'[mea culpa](/blog/categories/mea-culpa/)', and it's criminal 
code written by yours truly.

Behold the awesome that is `error()`, an unholy concoction of PHP, JavaScript, 
and HTML.  It's the only function in a file called common.php, which makes 
perfect sense because shut up.  Yes, this was in production use, and would 
dislpay a [JavaScript alert](http://www.w3schools.com/jsref/met_win_alert.asp) 
with an error message whenever I remembered to use it.

``` php
<?php // common.php
 
function error($msg) {
    ?>
    <html>
    <head>
    <script language="JavaScript">
    <!--
        alert("<?=$msg?>");
        history.back();
    //-->
    </script>
    </head>
    <body>
    </body>
    </html>
    <?
    exit;
}
```

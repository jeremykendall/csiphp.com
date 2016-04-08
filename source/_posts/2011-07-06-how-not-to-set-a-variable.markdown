---
author: jeremykendall
comments: true
date: 2011-07-06 15:45:12
layout: post
slug: how-not-to-set-a-variable
title: How not to set a variable
wordpress_id: 54
categories:
- PHP
author: "Jeremy Kendall"
---

This is the wrong way to set a variable . . .
``` php
<?php
foreach ($_POST as $Key => $Value) {
    // . . . 
    $values .= "'$Value',";
    // . . .
}
```

. . . unless you really like unexpected behavior and notice messages.  This particular app throws notice after notice after notice.  It throws so many, in fact, that the vertical scroll bar on many pages is just a tiny little sliver for all the [Xdebug](http://xdebug.org/) output.

**Pro Tip**: develop with [error reporting](http://php.net/manual/en/function.error-reporting.php) turned way, way up. (I usually use `error_reporting(-1)` in development.)

**UPDATE**: I updated to code sample based on [Dave's comment](http://csiphp.com/blog/2011/07/06/how-not-to-set-a-variable/#comment-33) that I may have introduced a bug during copy and paste.  Indeed I had.  Pop quiz: What do you think happens to `$values` after the `foreach()` exits?

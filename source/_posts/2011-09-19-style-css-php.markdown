---
author: jeremykendall
comments: true
date: 2011-09-19 09:03:34
layout: post
slug: style-css-php
title: style.css.php
wordpress_id: 334
categories:
- Education
- PHP
author: "Jeremy Kendall"
---

The concept makes sense - [minify static assets](http://en.wikipedia.org/wiki/Minification_(programming)) in order to make your pages smaller and therefore more responsive.  The execution leaves quite a lot to be desired.

```html+php
<?php

// minifying the CSS
header('Content-type: text/css');
ob_start("compress");
 
function compress($buffer) {
    // remove comments
    $buffer = preg_replace('!/\*[^*]*\*+([^/][^*]*\*+)*/!', '', $buffer);
    // remove tabs, spaces, newlines, etc.
    $buffer = str_replace(array("\r\n", "\r", "\n", "\t", '  ', '    ', '    '), '', $buffer);
    return $buffer;
}
?>

body {
    background-color: #FFF;
    color: black;
    margin: 0;
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-size: 10px;
}

table {
    margin: 0;
    padding: 0;
    border: 0;
}

// . . . snip . . .

<?php ob_end_flush(); ?>
```

Here are a few better ideas:
	
* Don't minimize anything at all. Is your site really that highly trafficked?
* Manually minimize your CSS and JavaScript files using freely [available](http://www.ventio.se/tools/minify-css/) [tools](http://www.refresh-sf.com/yui/).
* Combine and minify your static assets during the build process using something like [Phing](http://www.phing.info/trac/).

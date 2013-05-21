---
author: jeremykendall
comments: true
date: 2012-02-23 10:12:30
layout: post
slug: concatenation-is-not-a-parser-error
title: Concatenation is not a parser error
wordpress_id: 555
categories:
- PHP
---

CSI: PHP isn't big on the [perp walk](http://en.wikipedia.org/wiki/Perp_walk), but if your crime is (1) public and (2) licensed with an [Attribution-NonCommercial-ShareAlike](http://creativecommons.org/licenses/by-nc-sa/3.0/us/) Creative Commons license, then you kinda [perp walked yourself](http://jayjohnston.com/php/possible-php-parser-error).
    
```php
<?php

// What will this print out in php5?
$earth = 'World';
$string1 = "Hello " .
$string2 = $earth . '!';
$string = $string1 . $string2;

echo $string;
```

The author posits the output, `Hello World!World!`, is a possible parser error or internal assignment nonsense.  What `Hello World!World!` actually represents is PHP string handling and variable assignment 101.

I will concede that determining the output of `$string` is something of a light brain teaser.  However, if you're confused by concatenation and variable assignment to the point that you [write a blog post about it](http://jayjohnston.com/php/possible-php-parser-error) and present it as a parser error, I can only hope and pray you're trolling.

Exit question, "If a plane crashes on the border of Canada and the United States, where do you bury the survivors?"

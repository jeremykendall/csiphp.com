---
author: jeremykendall
comments: true
date: 2012-01-20 07:30:59
layout: post
slug: no-register_globals-no-problem
title: No register_globals, no problem
wordpress_id: 504
categories:
- PHP
author: "Jeremy Kendall"
---

Today's entry comes courtesy of CSI: PHP Investigator [Westin Shafer](https://twitter.com/#!/Hoggle_).

> "So I found this today in a commercial product that we purchased and I know you'll want to pull your eyes out their sockets as I did when I found this.  The script was originally written for php4 and now they've gone through the refactoring to make this php 5 compliant ::cough::.   This script apparently relied on [register_globals](http://us.php.net/manual/en/ini.core.php#ini.register-globals) to be set (which most php apps at the time did).  No problems, per-say, it was the trend back then... But now that PHP 5 is out and register_globals is no more, here's this guys solution to fix the problem...  Lazy like.

Who needs register globals when you can just mock up your own?"
    
```php
<?php
function get_all_vars() {
    $array=array('_GET', '_POST', '_SESSION', '_COOKIE');
    foreach($array as $key => $var) {
        global $$var;
        foreach ($$var as $k => $v) {
            global $$k;
            $$k=$v;
        }
    }
}
``` 

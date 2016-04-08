---
author: jeremykendall
comments: true
date: 2011-08-17 06:45:24
layout: post
slug: more-fun-with-database-input-filtering
title: More fun with database input filtering
wordpress_id: 228
categories:
- PHP
author: "Jeremy Kendall"
---

And I thought [`prepString()`](http://csiphp.com/blog/2011/08/12/i-do-not-think-this-function-does-what-you-think-it-does/) was the pinnacle of awesome.

```php
<?php

/* addslashes_mssql and stripslashes_mssql enable us to read from MS SQL Server 
(as opposed to other databases, where the PHP built-in functions addslashes() 
and stripslashes() work for MySQL and others
    */

function addslashes_mssql($str) {
    if (is_array($str)) {
        foreach ($str AS $id => $value) {
            $str[$id] = addslashes_mssql($value);
        }
    } else {
        $str = str_replace("'", "''", $str);
    }
    return $str;
}

function stripslashes_mssql($str) {
    if (is_array($str)) {
        foreach ($str AS $id => $value) {
            $str[$id] = stripslashes_mssql($value);
        }
    } else {
        $str = str_replace("''", "'", $str);
    }
    return $str;
}
``` 

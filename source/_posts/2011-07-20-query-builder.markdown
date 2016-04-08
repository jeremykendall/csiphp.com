---
author: jeremykendall
comments: true
date: 2011-07-20 07:20:59
layout: post
slug: query-builder
title: Query builder
wordpress_id: 101
categories:
- PHP
author: "Jeremy Kendall"
---

This is so bad ass.  I'm going to start using it in all my projects.  Wait, it's already in all my projects.  FML.

```php
<?php 
Function BuildQuery() {
    global $SelectValue;
    global $FromValue;
    global $WhereValue;
    global $OrderValue;

    $dbquery = $SelectValue;
    $dbquery .= $FromValue;
    $dbquery .= $WhereValue;
    if ($OrderValue) {
        $dbquery .= $OrderValue;
    }
    return($dbquery);
}
``` 

Bonus snark: Language construct?  What's a [language construct](http://php.net/manual/en/function.return.php)?

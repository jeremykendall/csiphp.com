---
author: jeremykendall
comments: true
date: 2011-08-15 09:45:55
layout: post
slug: unleashing-the-power-of-the-boolean
title: Unleashing the power of the boolean
wordpress_id: 221
categories:
- PHP
---

Now witness the firepower of this fully ARMED and OPERATIONAL boolean conversion function! 

```php
<?php
Function _toBoolean($str) {
    return $str ? 1 : 0;
}

// Use case:
$obj->set(DB::_toBoolean($arr['myIndex']));
```    

> "This function is soooo versatile. It takes care of non-existing indexes in databases, strings, ... and converts them all to a boolean. Never mind the meaning of boolean."  -- Anonymous submitter





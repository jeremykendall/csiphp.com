---
author: jeremykendall
comments: true
date: 2011-08-10 10:44:13
layout: post
slug: database-abstraction-at-not-its-finest
title: Database abstraction at not its finest
wordpress_id: 213
categories:
- PHP
---

I see what you're trying to do here, and I commend you for your creativity in problem solving, but I can't help but think you're doing it wrong.

```php
<?php 

// MSSQL SQL string for getting top 5 rows from a query
function top($value)
{
    if (DB_SERVER == 'MSSQL') {
        $string = " TOP " . $value . " ";
    } elseif (DB_SERVER == 'MYSQL') {
        $string = "";
    } else {
        die('Error: Database server not supported !');
    }
    return $string;
}

// MYSQL SQL string for getting top 5 rows from a query
function limit($value)
{
    if (DB_SERVER == 'MSSQL') {
        $string = "";
    } elseif (DB_SERVER == 'MYSQL') {
        $string = " LIMIT 0, " . $value . " ";
    } else {
        die('Error: Database server not supported !');
    }
    return $string;
}
```

---
author: jeremykendall
comments: true
date: 2011-07-06 08:22:47
layout: post
slug: really-preg_replace
title: Really? preg_replace()?
wordpress_id: 46
categories:
- PHP
---

I found this in a database connection config file.  How many problems do you see with this?  I'll give you a head start -- [preg_replace()](http://us.php.net/preg_replace) is overkill.  And by overkill, I mean "absolutely unnecessary".
``` php
<?php
// . . .
$MM_sql_HOSTNAME = 'hostname';
$MM_sql_DATABASE = 'mssql:dbname';
$MM_sql_DBTYPE = preg_replace('/:.*$/', '', $MM_sql_DATABASE);
$MM_sql_DATABASE = preg_replace('/^[^:]*?:/', '', $MM_sql_DATABASE);
$MM_sql_USERNAME = 'username';
$MM_sql_PASSWORD = 'secret';
// . . .
``` 

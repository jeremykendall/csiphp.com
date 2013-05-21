---
author: jeremykendall
comments: true
date: 2012-09-28 08:00:42
layout: post
slug: too-few-defines-for-my-taste
title: Too few DEFINEs for my taste
wordpress_id: 624
categories:
- PHP
---

##The Crime##
It takes a big man to admit when he's wrong, and [@dilbert4life](https://twitter.com/dilbert4life) is one of those men.  Here's a snippet of horror that he wrote around a year-and-a-half ago.  

Too many defines?  Nay!  I say let the site grow and see just how many we can stuff in there.
    
``` php
<?php
$slug = str_replace('.php', '', $_SERVER['REQUEST_URI']);
$slug = preg_replace('/\//', '', $slug, 1);
$slug = explode('?', $slug);
$slug = $slug[0];
if($slug == 'index' || $slug == '') {
    $slug = "/";
    define(IS_HOME, true);
    define(IS_ADMIN, false);
    define(IS_NEWS, false);
    define(IS_CONTACT, false);
} elseif(preg_match('/admin/', $slug)) {
    define(IS_HOME, false);
    define(IS_ADMIN, true);
    define(IS_NEWS, false);
    define(IS_CONTACT, false);
} elseif(preg_match('/news/', $slug)) {
    define(IS_HOME, false);
    define(IS_ADMIN, false);
    define(IS_NEWS, true);
    define(IS_CONTACT, false);
} elseif(preg_match('/contact/', $slug)) {
    define(IS_HOME, false);
    define(IS_ADMIN, false);
    define(IS_NEWS, false);
    define(IS_CONTACT, true);
} else {
    define(IS_HOME, false);
    define(IS_ADMIN, false);
    define(IS_NEWS, false);
    define(IS_CONTACT, false);
}
```

##Rehabilitation##
Normally, a submitter, whether anonymous or not, will simply send in an incident report and leave it at that.  @dilbert4life has gone above and beyond, however.  Not only did he submit his crime, but he submitted evidence of his rehabilitation.

``` php
<?php
$uri = parse_url($_SERVER['REQUEST_URI']);
$path = explode('/', $uri['path']);

define('IS_HOME', (empty($path[1])));
define('IS_ADMIN', ($path[1] === 'admin'));
define('IS_NEWS', ($path[1] === 'news'));
define('IS_CONTACT', ($path[1] === 'contact'));
```   

I especially like how he replaced three lines of garbage with `parse_url`.

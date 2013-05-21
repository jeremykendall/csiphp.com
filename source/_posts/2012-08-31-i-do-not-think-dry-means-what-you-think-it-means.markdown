---
author: jeremykendall
comments: true
date: 2012-08-31 08:00:04
layout: post
slug: i-do-not-think-dry-means-what-you-think-it-means
title: I do not think DRY means what you think it means
wordpress_id: 572
categories:
- PHP
---

Perhaps this developer decided it would be better to create convenience methods than to litter their codebase with date format strings.  Nothing wrong with trying to [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) up your code, but creating 13 inscrutably named date formatting functions ain't the way to do it.

```php
<?php

function formatDate1($date)
{
    return date('n-j-y', $date);
}

function formatDate2($date)
{
    return date('n/j/Y', $date);
}

function formatDate2a($date)
{
    return date('n/j/Y g:ia', $date);
}

function formatDate2b($date)
{
    return date('n.j.Y g:ia', $date);
}

function formatDate3($date)
{
    return date('n/j/y', $date);
}

function formatDate11($date)
{
    return date('n/j', $date);
}

function formatDate4($date)
{
    return date('F j, Y', $date);
}

function formatDate5($date)
{
    return date('F j, Y  g:i:s A', $date);
}

function formatDate6($date)
{
    return date('j-M', $date);
}

function formatDate8($date)
{
    return date('F j, Y  g:i A', $date);
}

function formatDate9($date)
{
    return date('n.j.Y', $date);
}

function formatDate10($date)
{
    return date('M Y', $date);
}

function formatDate12($date)
{
    return date('m/d/Y', $date);
}
```

I was able to confirm, thanks to further research by today's anonymous submitter, that at least half of these functions are currently in use.

[_**EDIT**: The wikipedia link wasn't encoded properly when I posted this. Fixed! Thanks to [@papayasoft](https://twitter.com/papayasoft) for pointing it out._]

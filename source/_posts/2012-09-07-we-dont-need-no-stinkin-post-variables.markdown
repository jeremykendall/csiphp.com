---
author: jeremykendall
comments: true
date: 2012-09-07 08:00:09
layout: post
slug: we-dont-need-no-stinkin-post-variables
title: We don't need no stinkin' POST variables
wordpress_id: 590
categories:
- PHP
author: "Jeremy Kendall"
---

CSI: PHP investigator [Duane Gran](https://twitter.com/duanegran) sent in this horrifying snippet.  He explains:

> I wondered why dumping the `$_POST` variables before this section didn't help 
> in debugging.  This occurs in a second step of a 3-step form on a `GET` request. 
> It applies a set of session fields to the `$_POST` variable for later use.
    
```php
<?php

foreach($_SESSION["purchase"] as $key => $value)
{
    switch($key)
    {	
        case "process_purchase":
            break;
        default:
            $_POST[$key] = $value;
            break;
    }
}
``` 

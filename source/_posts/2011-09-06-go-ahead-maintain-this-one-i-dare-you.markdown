---
author: jeremykendall
comments: true
date: 2011-09-06 10:36:09
layout: post
slug: go-ahead-maintain-this-one-i-dare-you
title: Go ahead. Maintain this one. I dare you.
wordpress_id: 286
categories:
- PHP
---

Page titles are stored in a numerically indexed session array.  There are 22 cases in the switch statement and no default case.  The style_inc property sometimes contains javascript, but not always.  

Go ahead.  Maintain this one.  I dare you. I _double-dog_ dare you.

```php
<?php

class Page
{
    // . . .

    function set_style_title($page, $i)
    {
        switch ($page) {
            case 'home':
                $this->title = $_SESSION['content_title'][$i];
                $this->style_inc = '<link href="media/style/common.css" type="text/css" rel="stylesheet"></link>
                                    <link href="media/style/home.css" type="text/css" rel="stylesheet"></link>';
                break;
            case 'lost_pass':
                $this->title = $_SESSION['content_title'][$i];
                $this->style_inc = '<link href="media/style/common.css" type="text/css" rel="stylesheet"></link>
                                    <link href="media/style/lost.css" type="text/css" rel="stylesheet"></link>';
                break;
            case 'intro':
                $this->title = $_SESSION['content_title'][$i];
                $this->style_inc = '<link href="media/style/common.css" type="text/css" rel="stylesheet"></link>
                                    <link href="media/style/intro.css" type="text/css" rel="stylesheet"></link>';
                break;

            // . . . 19 more cases, but no default case . . .
        }
    }
    
    // . . .
}
``` 

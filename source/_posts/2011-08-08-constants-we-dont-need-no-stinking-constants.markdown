---
author: jeremykendall
comments: true
date: 2011-08-08 07:22:04
layout: post
slug: constants-we-dont-need-no-stinking-constants
title: Constants? We don't need no stinking constants!
wordpress_id: 202
categories:
- PHP
---

[`PHP_MAJOR_VERSION`](http://php.net/manual/en/reserved.constants.php)?  Screw it, I'll do it myself.

```php
<?php

if (floor(phpversion()) >= 5)
{
    $this->load =& load_class('Loader');
    $this->load->_ci_autoloader();
}
``` 

Just to be clear, there's nothing _necessarily_ criminal about today's code. Rather, it's an example of code that could be written in a cleaner, more efficient manner.  Today's lesson might be, "Knowing your language is the path to better code."

Now you know about `PHP_MAJOR_VERSION`, [and knowing is half the battle](http://www.youtube.com/watch?v=pele5vptVgc).

Today's snippet comes courtesy of an anonymous submitter.

**UPDATE**: Yeah, remember what I said about "Knowing your language . . ."?  Well, I just got schooled.  Commenter [Josh Woody](http://csiphp.com/blog/2011/08/08/constants-we-dont-need-no-stinking-constants/#comment-164) demonstrates how using `PHP_MAJOR_VERSION` wouldn't really be any better, and his point is well taken.  Since `PHP_MAJOR_VERSION` has only been available since PHP 5.2.7, it wouldn't be much use in any version prior to 5.2.7. /facepalm.

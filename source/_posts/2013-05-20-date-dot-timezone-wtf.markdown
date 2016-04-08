---
layout: post
title: "date.timezone WTF?"
date: 2013-05-20 23:55
comments: true
categories: PHP
author: "Jeremy Kendall"
---

CSI: PHP Investigator [@jsundquist](https://twitter.com/jsundquist) recently forwarded 
the below incident report:

> Yesterday a user posted in the php general mailing list asking why he needed 
> to set a default timezone now for php 5.3.  It was explained to him why and how.  
> After doing some of his own research he decided to go the route of:

```php
<?php

date_default_timezone_set(@date_default_timezone_get());
```

>His only reasoning for wanting to go this route is because it is "deployed on servers all over the world and he does not know what timezone the server is in".
>
> All I can say to that is, wow. Set it to UTC and leave it at that.

I'm with Sundquist on this one.  Wow.

## Timezone is serious business ##

First, always set `date.timezone`. Always, _always_, **always**.  It's used by all of the 
PHP date/time functions, and those are pretty damned important.

If the timezone is invalid, PHP generates `E_NOTICE`. You'll get an `E_WARNING` if PHP has to guess your timezone 
from the system settings or has to use the TZ environment variable. What's more, 
PHP no longer attempts to guess your timezone as of PHP `5.4.0`. Setting your 
timezone in your `php.ini` or using `date_default_timezone_set()` is a must.

## The "shutup" operator ##

Don't use it.  Ever.  It's never OK to use the shutup operator.  Those notices 
and warnings are there for a reason, and should be dealt with immediately (you 
always develop with `error_reporting` set to `-1`, right?). 

Worse than silencing notices and warnings, the 
[Error Control Operators documentation](http://php.net/manual/en/language.operators.errorcontrol.php) states, 

> ". . . the "@" error-control operator prefix will even disable error 
> reporting for critical errors that will terminate script execution." 

Good luck troubleshooting that business.

## So which timezone do I use? ##

Take Sundquist's advice.  If you're not sure what to do with your timezone, set it to 
`UTC` and move on.  

If you have any thoughts or advice you'd like to add, drop 'em in the comments below.

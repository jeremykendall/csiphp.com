---
author: jeremykendall
comments: true
date: 2012-01-06 07:30:17
layout: post
slug: templating-gone-horribly-wrong
title: Templating gone horribly wrong
wordpress_id: 468
categories:
- PHP
---

Using any form of templating, even if it's just including a header.php and footer.php in each page, is supposed to make a developer's life easier.  Imagine the confusion the below code causes when, as you're reviewing some new code, you run across a seemingly random `</head>` tag.  Where is the rest of the markup, you might ask?  Why, take a look at `doPageHead()`, silly.
    
```php
<?php

function doPageHead($lang='en') {

    print "
    <html lang="$lang" xmlns="http://www.w3.org/1999/xhtml" xml:lang="$lang">
    <head>
        <meta content="text/html; charset=utf-8" http-equiv="Content-Type"></meta>
    ";
}
```

I'm not a big fan of functions that simply `echo` HTML, but if you're going to do something like this, at least include the closing head tag.  Seriously.  Or I might come find you and punch you square in the jeans.

Another, better option is to find a [PHP templating engine](https://www.google.com/search?q=php+templating+engine&ie=utf-8&oe=utf-8&aq=t&rls=org.mozilla:en-US:official&client=firefox-a#sclient=psy-ab&hl=en&client=firefox-a&hs=Ira&tbo=1&rls=org.mozilla:en-US%3Aofficial&tbs=qdr:y&source=hp&q=php+templating+engine&pbx=1&oq=php+templating+engine&aq=f&aqi=&aql=&gs_sm=s&gs_upl=0l0l0l61329l0l0l0l0l0l0l0l0ll0l0&tbo=1&bav=on.2,or.r_gc.r_pw.r_cp.,cf.osb&fp=4b3c420078196b39&biw=1366&bih=590) that'll do most of your templating for you.

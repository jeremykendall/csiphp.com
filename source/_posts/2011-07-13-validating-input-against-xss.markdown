---
author: jeremykendall
comments: true
date: 2011-07-13 07:25:41
layout: post
slug: validating-input-against-xss
title: Validating input against XSS
wordpress_id: 96
categories:
- PHP
---
The intent is self explanatory. The results are likely to be less than desirable.  I'm pretty sure this won't cover all possible use cases.

``` php
<?php
Function noScript($text) {
    if (preg_match_all('/http:\/\/pagead2.googlesyndication.com\/pagead\/show_ads.js/i', "$text", $ntext)) {
        $text = str_replace($ntext[0], '', $text);
    }
    if (preg_match_all('/<script type="text\/javascript"/i', "$text", $ntext)) {
        $text = str_replace($ntext[0], '<!--', $text);
    }
    if (preg_match_all('/<\/script>/i', "$text", $ntext)) {
        $text = str_replace($ntext[0], '-->', $text);
    }
    return $text;
}
```

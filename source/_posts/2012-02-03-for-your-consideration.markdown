---
author: jeremykendall
comments: true
date: 2012-02-03 14:30:39
layout: post
slug: for-your-consideration
title: For your consideration
wordpress_id: 527
categories:
- PHP
---

This was sent along anonymously, along with the question:

> "Does this count as horror code or pure evil genius code?"

What say you, dear reader?  I cut and paste, you decide.

```php
<?php
 
function cli_parsestr($string, $config, $mainconf, $options, $custom = array())
{
    if (empty($string)) {
        return '';
    }

    $replaces = array(
            'config' => '/%%$0%%/',
            'mainconf' => '/%\*$0\*%/',
            'options' => '/%&$0&%/',
            'custom' => '/%\\\$$0\\\$%/',
            );

    foreach ($replaces as $var => $replace) {
        if (!empty($$var) && is_array($$var)) {
            $string = preg_replace(preg_replace('/.+/', $replace, array_keys($$var)), array_values($$var), $string);
        }
    }
    return $string;
}
```

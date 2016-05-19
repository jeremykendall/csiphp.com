---
layout: post
title: "stop writing your own strip tags"
date: 2016-05-02 21:10
comments: true
categories: [ "PHP", "Security", "Not Invented Here" ]
author: "Joe Ferguson"
---

[@devnuhl](https://twitter.com/devnuhl) sent this one to us via a Gist earlier today;

Some background he gave us:

> Honestly just stumbled onto this while updating the codebase. Oddly enough, it seems like the only usage of this function is in another function in the same file, which is being added to the gist now. Having gotten rid of some glaring errors in the regular expressions, I find it interesting that preg_replace is used in the one, but not the other. Just replace \s+ with a space.

Here is the original code:

``` php
<?php
#...
if (function_exists('strip_html_tags') === FALSE) {
    function strip_html_tags($str) {
        $replacements = array(
            '#<head[^>]*?>.*?</head>#siu',
            '#<style[^>]*?>.*?</style>#siu',
            '#<script[^>]*?>.*?</script>#siu',
            '#<noscript[^>]*?>.*?</noscript>#siu',
        );
        $str = preg_replace('/(<|>)\1{2}/is', '', $str);
        $str = preg_replace($replacements, "", $str);
        $str = replace_whitespace($str);
        $str = strip_tags($str);
        return $str;
    }
}
if (function_exists('replace_whitespace') === FALSE) {
    function replace_whitespace($str) {
        $result = $str;
        $empties = array(
            " \t",
            " \r",
            " \n",
            "\t\t",
            "\t ",
            "\t\r",
            "\t\n",
            "\r\r",
            "\r ",
            "\r\t",
            "\r\n",
            "\n\n",
            "\n ",
            "\n\t",
            "\n\r",
        );
        foreach ($empties as $replacement) {
            $result = str_replace($replacement, ' ', $result);
        }
        return ($str !== $result) ? replace_whitespace($result) : $result;
    }
}
```

The only hope I have for the original author is that they wrote this to fight back against some ridiculous encoding issues when it came to whitespace.

Assuming you don't have ridiculous encoding issues here are some better alternatives for replacing whitespace:

### Trim ([PHP Docs](http://php.net/manual/en/function.trim.php))

``` php
<?php
if (function_exists('replace_whitespace') === FALSE) {
    function replace_whitespace($str) {

        return trim($str);
    }
}
```

You do not need a method to do this, you can simly `trim($str)` elsewhere in your application

As the docs state, without a second parameter `trim()` will strip these characters:

* " " (ASCII 32 (0x20)), an ordinary space.
* "\t" (ASCII 9 (0x09)), a tab.
* "\n" (ASCII 10 (0x0A)), a new line (line feed).
* "\r" (ASCII 13 (0x0D)), a carriage return.
* "\0" (ASCII 0 (0x00)), the NUL-byte.
* "\x0B" (ASCII 11 (0x0B)), a vertical tab.

### String Replace ([PHP Docs](http://php.net/manual/en/function.str-replace.php)) or Preg Replace ([PHP Docs](http://php.net/manual/en/function.preg-replace.php))

Again with this method, you do not need to pass this into it's own method.

To remove a space " " (ASCII 32 (0x20)):

```php
<?php

$str = str_replace(' ', '', $str);
```

To remove all whitespace, we can use a regular expression:

```php
<?php

$str = preg_replace('/\s+/', '', $str);
```

More discussion found at [stackoverflow](http://stackoverflow.com/questions/2109325/how-to-strip-all-spaces-out-of-a-string-in-php)

### Recap

Rolling your own functionality that PHP already provides is silly. If you believe you need to do this; do some searching. You've probably either found an edge case or you need to do some refactoring.


Thanks for Reading. Happy Coding!

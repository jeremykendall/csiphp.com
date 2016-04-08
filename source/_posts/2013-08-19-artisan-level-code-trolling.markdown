---
layout: post
title: "Artisan Level Code Trolling"
date: 2013-08-19 13:19
comments: true
categories: [ "PHP", "Trolling", "Humor" ]
author: "Jeremy Kendall"
---
*WARNING: The code you're about to view is intended for mature audiences and
may not be suitable for all readers.*

I saw this *horrifying* code snippet in my Twitter feed this afternoon.

{% tweet https://twitter.com/grhmc/status/369512635814785024 %}

To make this abomination easier to read, I've copied and formatted it here.

``` php
<?
foreach (get_defined_vars() as $var) { 
    @extract($var); 
}
```

What this nightmare does, in case you aren't immediately able to grok it, is iterate over
[a multidimensional array containing a list of all defined variables](http://php.net/get_defined_vars)
(environment, server, and user defined) and [import them](http://php.net/extract) into
the current [symbol table](http://en.wikipedia.org/wiki/Symbol_table). If that weren't bad enough,
it uses the [@ error control operator](http://php.net/manual/en/language.operators.errorcontrol.php)
and (because this is extract's default behavior) overwrites any existing defined variable
it encounters.

Try and debug unexpected behavior with that in your code.  Go ahead.  Try. You'll be
reduced to a wimpering pile of crazy in just a few short hours.

{% youtube AG7LjVCj50Y %}

There simply aren't words sufficient to express the horror of this snippet.  The worst
thing about this is I've seen similar code in the wild, and so has [Graham](https://twitter.com/grhmc).

{% tweet https://twitter.com/grhmc/status/369515635023155201 %}

Looks like [osCommerce](http://www.oscommerce.com/) might have some 'splainin' to do . . .

It's a cold, cruel world of bad code sometimes.  I'm sorry I had to share this with
you.  Hopefully it helps more than it hurts, since knowing what bad code looks
like is one of the first steps in being able to fix it.

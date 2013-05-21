---
author: jeremykendall
comments: true
date: 2011-08-29 19:08:56
layout: post
slug: i-think-im-going-to-be-ill
title: I think I'm going to be ill . . .
wordpress_id: 275
categories:
- PHP
---

Yup, I just threw up in my mouth a little.

```php
<?php

foreach ($this->models as $model) {
    $_file = '../' . $this->app . 'Models/' . $model . 'Model.php';
    $_class = $model . 'Model';

    if (file_exists($_file)) {
        require_once $_file;
        $this->{ucfirst($model)} = new $_class(null, dbConfig::$default);
    } else {
        $class = 'class ' . $_class . ' extends Model { }';
        eval($class);
        $this->{ucfirst($model)} = new $_class(null, dbConfig::$default);
    }
}
```

[Source](https://gist.github.com/1158943)

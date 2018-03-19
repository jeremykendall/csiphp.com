---
author: jeremykendall
comments: true
date: 2011-07-07 10:10:33
layout: post
slug: oh-thats-where-the-mail-function-is-called
title: Oh. That's where the mail function is called.
wordpress_id: 67
categories:
- PHP
author: "Jeremy Kendall"
---
```php
<?php
eval( ''$RuleRes = cf_MailInsert($row,$id);'' )
```
I still haven't found where in the database the eval'd string is coming from.  FML.

For background see [Pure evil](/blog/2011/07/07/pure-evil/).  This function call is the result of the `eval("$doAction" . ";");` line.

---
author: jeremykendall
comments: true
date: 2011-07-11 07:15:04
layout: post
slug: doposteval
title: doPostEval()
wordpress_id: 80
categories:
- PHP
---

Post as in "after", yes, but also post as in [`$_POST`](http://www.php.net/manual/en/reserved.variables.post.php).  Eval as in our old enemy [`eval()`](http://us.php.net/eval).  This function is called on every single `$_POST` key/value pair after form submission.  `$On_Insert` is an array of executable PHP code stored in the database.

Figure out the rest of the misery yourself.  I might jump off a cliff if I have to think through this code again.

```php
<?php
Function doPostEval($Key,$Value,$On_Insert)
{
    $Code = $On_Insert[$Key];
    $xxx = '';
    if ($Code <> '') {
        eval($Code);
        return($xxx);
    } else {
        return($Value);
    }
}
```

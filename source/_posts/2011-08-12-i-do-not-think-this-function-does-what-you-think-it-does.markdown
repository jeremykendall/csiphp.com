---
author: jeremykendall
comments: true
date: 2011-08-12 09:28:02
layout: post
slug: i-do-not-think-this-function-does-what-you-think-it-does
title: I do not think this function does what you think it does
wordpress_id: 216
categories:
- PHP
---

How 'bout a little data filtering before you go inserting user-generated data into that nice database of yours?  Here's a handy function that does just that.  No need to test this one; I know it outputs _exactly_ what I expect.

```php
<?php

Function prepString($string) { // Specifically for MSSQL '' instead of \'
    $badChars = array("'", "&", ",");
    $goodChars = array("", "&", "");

    if (is_string($string)) {
        $string = str_replace($badChars, $goodChars, $string);
        $string = stripslashes($string);
        $string = "$string";
    }

    return($string);
}
``` 

Here's an example of the function in use.

```php    
<?php

foreach ($_POST as $x => $y) {
    $_POST[$x] = prepString($y);
}

$query = "insert into table (column1, column2, column3, column4)
    values('" . $_POST['input1'] . "','" . $_POST['input2'] 
    . "', '" . $_POST['input3'] . "', '" . $_POST['input4'] . "')";
```

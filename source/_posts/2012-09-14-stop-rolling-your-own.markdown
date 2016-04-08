---
author: jeremykendall
comments: true
date: 2012-09-14 08:00:29
layout: post
slug: stop-rolling-your-own
title: Stop rolling your own
wordpress_id: 604
categories:
- PHP
author: "Jeremy Kendall"
---

Seriously.  Stop it.  Or you'll end up with garbage like this, in which a developer 
writes _two separate functions_ for converting [JSON](http://www.json.org/) to 
an array, only one of which is compatible with [`json_decode`](http://php.net/manual/en/function.json-decode.php). 
Yes, both are in production.
    
```php
<?php

function jsonToArray($jsonData)
{

    $newArray = array();

    // remove first and last []
    $length = strlen($jsonData) - 2;
    $jsonString = substr($jsonData, 1, $length);

    if ($jsonString != "") {
        // tokenize, using , as a divider
        $newArray = explode(",", $jsonString);
    }
    return $newArray;
}

// Converts JSON data string to PHP array
// Assumes this format: ["Some text","Even more text"]
function jsonStringsToArray($jsonData)
{

    // remove first and last []
    $length = strlen($jsonData) - 2;
    $jsonString = substr($jsonData, 1, $length);

    // tokenize, using , as a divider
    $newArray = explode(",", $jsonString);

    $lastArray = array();

    for ($i = 0; $i < sizeof($newArray); $i++) {

        $thisLength = strlen($newArray[$i]) - 2;
        $lastArray[] = substr($newArray[$i], 1, $thisLength);
    }
    return $lastArray;
}
```

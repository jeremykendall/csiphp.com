---
author: jeremykendall
comments: true
date: 2011-08-19 11:17:05
layout: post
slug: designed-to-be-called-in-the-middle-of-an-sql-query
title: Designed to be called in the middle of an SQL query
wordpress_id: 234
categories:
- PHP
author: "Jeremy Kendall"
---

". . . to simplify both the readability and repeatability of the query."  Yeah, I'm not sure it does that, especially when your function's comments are TL;DR.
    
```php
<?php

Function SQLMonthRange($field, $month=0, $formatOut="M", 
                        $relative=true, $between=true) {
    /*
    * This is designed to be called in the middle of an SQL query
    * to simplify  both the readability and repeatability of the 
    * query.  The SQL query is assumed to be calling to capture 
    * values for a given field ($field) during 
    * the given month ($month).
    * 
    * A sample call might be:
    * 
    * $months =SQLMonthRange('COLUMN','11','M');
    * 
    * A sample output might be:
    * 
    * ( COLUMN BETWEEN '2007-Nov-01' AND '2007-Dec-01' )
    * 
    * when using the default $between = true, or
    * 
    * ( COLUMN >= '2007-Nov-01' and COLUMN < '2007-Dec-01' )
    * 
    * when using $default = false
    * 
    * $month is any valid (positive or negative) integer.  
    * In relative mode ($relative=true), 0 is the current month.
    * In absolute mode ($relative=false) 0 is December 
    * of the previous year
    *
    * RELATIVE MODE
    * $relative (true by default) allows us to keep a sliding month 
    * scale so that either positive or negative values reflect the 
    * current month plus the value of $month.  For example, if the 
    * present month is March, a $month value of 
    * -2 refers to January (two months prior to March), and a 
    * $month value of 5 refers to August (5 months after March).
    * 
    * Be careful in absolute mode (where $relative is false).  
    * In absolute mode, a $month value of -1 is NOT December.  
    * Remember that since 1 is January, it follows that 0 must 
    * be December (again, this example is for absolute mode).
    * 
    * $month =   0 ==> December of the previous year in absolute mode ($relative=false)
    * $month =  12 ==> December of the current year in absolute mode ($relative=false)
    * $month = -11 ==> January of the previous year in absolute mode ($relative=false)
    * 
    * $month =   0 ==> current month in relative mode ($relative=true)
    * $month =  12 ==> 12 months ahead of current month in relative mode ($relative=true)
    * $month = -11 ==> 11 months prior to current month in relative mode ($relative=true)
    *  
    * BETWEEN
    * $between allows us to switch to a BETWEEN statement or use 
    * the >= / <= combination, which may be useful when comparing 
    * speed tests.  Also, BETWEEN is inclusive of the beginning 
    * and ending values.  That is, it does in fact have an implicit 
    * ">=" at the beginning value and a "<=" at the ending value 
    * (not just a ">" and a "<").
    // http://msdn2.microsoft.com/en-us/library/aa225976(SQL.80).aspx

    */

    // ... snip ...
}
``` 

If you're brave enough, or have enough time to waste, you can see the [full function code here](http://pastebin.com/Qwakm4Qz).

Use case:
    
```php
<?

$q = "SELECT * FROM table WHERE SQLMonthRange " 
        . SQLMonthRange('column', 7, 'm');
``` 

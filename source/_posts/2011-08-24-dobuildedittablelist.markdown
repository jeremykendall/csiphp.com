---
author: jeremykendall
comments: true
date: 2011-08-24 10:42:41
layout: post
slug: dobuildedittablelist
title: doBuildEditTableList
wordpress_id: 252
categories:
- PHP
author: "Jeremy Kendall"
---

My favorite parts are the relative links in to the stylesheet and the image. Write once, use only in a very specific subdirectory. Genius.

```php
<?php

Function doBuildEditTableList($dbn){

    require "../config.inc";
    global $conn;
 
    $dbquery = "SELECT mytablename FROM updatetables ";
    mssql_select_db($dbn, $conn);
    $result = mssql_query($dbquery);
        print "<html><head>";
        print "<link rel=stylesheet type=text/css href='../$stylesheet' >";
        print "";
        print "</head><body>";
        print "<a href=$web><img align=center height=24 width=75 src=..$img/home.jpg alt='Return to Menu'></a>";
        print"<table class='form' align='center' width=75%>";
        print"<tr><td align='center' class='arow'>Edit Tables</td></tr>";
        print"<tr><td align='center' class='brow'>Select the table that you want to update:</td></tr></table>";
        print"<table class='form' align='center' width=75%>";
        While ($row = mssql_fetch_assoc($result)){
            foreach($row as $Key => $Value){ //foreach($row as $value)
                print "<tr><td align='center' class='form'><a class='blacklink' href=$script?req=list&table=$Value>$Value</a></td></tr>";
            }
        }   
    print "</table></html>";
}
```

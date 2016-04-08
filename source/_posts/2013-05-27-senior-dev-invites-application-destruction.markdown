---
layout: post
title: "Senior dev invites application destruction"
date: 2013-05-27 12:48
comments: true
categories: 
- PHP
- MySQL
- "SQL Injection"
author: "Jeremy Kendall"
---

I'm not sure what else you'd call this besides an open invitation to face roll your 
database. How many times have we discussed SQL injection here at CSI: PHP? I guess we'll have to discuss it a lot more.

I'll let contributor [@dilbert](https://twitter.com/dilbert4life) for life 
explain it [in his own words](https://gist.github.com/dongilbert/4327713):

> "Read this today on a developer thread. I was horrified, but then I realized 
> he was working on his local machine - so no issue there<sup id="fnref:1">[1](#fn:1)</sup>. But, then I read the 
> rest of his email/question, and in his signature, it said "Senior PHP Developer." 
> NNNOOOOOO"

```php
<?php

mysql_connect("localhost", "uname", "pass@3") or die(mysql_error());

echo "Connected to MySQL<br />"; // working fine

mysql_select_db("dbname") or die(mysql_error());

echo "Connected to Database"; // working fine

extract($_POST);

$result = mysql_query("SELECT * FROM admin_users WHERE admin_email='" . $user_email .
        "' AND password='" . md5($password) . "' AND status=1") or die(mysql_error()); // not working returns null

while($row = mysql_fetch_array( $result )) {
    print_r ($row);
}
```

This is a straight education problem.  My suspicion is this "Senior" dev is the 
_only_ dev, has worked solo for many years, reads very little, and has no contact 
with the PHP community.  I wish there was a good way to reach out to those devs, 
show them the error of their ways, and teach them the right way. Ah, but I digress 
from my usual cynical snark.

WAT A BIG DUMMY.

With that out of the way, the right way to refactor the snippet above is to 1) 
use PDO and 2) use prepared statements. Show us how it's done, [PHP: The Right Way](http://www.phptherightway.com/#databases).

---
####Notes

<a id="fn:1"></a>1. I respectfully disagree with the contention that bad practices 
are acceptable in local environments.  "_Practice like you'll play_" is one of 
my favorite quotes. In this context it means that you should write every line of code as if it's going 
into production. Best practices are best practices _everywhere_.

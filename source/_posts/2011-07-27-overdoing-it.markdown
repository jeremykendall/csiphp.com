---
author: jeremykendall
comments: true
date: 2011-07-27 07:22:26
layout: post
slug: overdoing-it
title: Overdoing it
wordpress_id: 135
categories:
- PHP
author: "Jeremy Kendall"
---

I am not making this shit up.

```php 
<?php 
Function mail_attachment($from, $to, $subject, $message, $attachment) {
    . . .
    $email_from = $from; // Who the email is from
    $email_subject = $subject; // The Subject of the email
    $email_txt = $message; // Message that the email has in it
    $email_to = $to; // Who the email is to
    . . .
}
```

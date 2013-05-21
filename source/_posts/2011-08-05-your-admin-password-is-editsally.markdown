---
author: jeremykendall
comments: true
date: 2011-08-05 07:15:02
layout: post
slug: your-admin-password-is-editsally
title: Your admin password is editsally
wordpress_id: 188
categories:
- PHP
---

I'm not sure if this is what the original developer sent to the client once the app was finished, but I think I'm probably pretty close.

> Hey Sally,
>
> I got that application put together for you.  You're the application administrator, or course.  You don't need a username, and your password is 'editsally'.  Since the app's users are easily confused, we'll just let them all use the same password, 'companyuser'.
> 
> Best Regards,
> 
> Web Developer

And just how was this genius security plan implemented?

```php
<?php 
@session_start();

if (isset($_SESSION['pass'])) {

} else {
    if($_POST['pass'] == 'editsally') {
        $_SESSION['pass'] = 'editsally';
    } elseif($_POST['pass'] == 'companyuser') {
        $_SESSION['pass'] = 'companyuser';
    } else {
        $_SESSION['pass'] = $_SESSION['pass'];
    }
}

// . . . snip 150 lines of HTML and JavaScript . . .

if (isset($_SESSION['pass'])) {
    // "Secure" portion of page:
    // 160 terrifying lines of HTML, JavaScript, and PHP.
} else {
    echo('<form action="index.php" method="POST">Enter Password:<input type="password" name="pass"><input type="submit" value="Login"></form>');
}
``` 

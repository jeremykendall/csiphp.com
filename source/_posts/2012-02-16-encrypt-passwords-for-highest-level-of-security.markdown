---
author: jeremykendall
comments: true
date: 2012-02-16 07:30:47
layout: post
slug: encrypt-passwords-for-highest-level-of-security
title: Encrypt passwords for highest level of security
wordpress_id: 545
categories:
- Education
- PHP
---

Thanks to Justin Carmony for this awesome slice of fail.
    
```php
<?php

class SecurityFail
{

    // Encrypt Passwords for Highest Level of Security.
    static public function encrypt($pword)
    {
        return md5($pword);
    }

}
```

There are right ways and wrong ways to encrypt and store passwords, and a simple md5() hash is one of the wrong ways. Here are some links you might research instead of rolling your own crypto.
	
* See this Stack Overflow discussion regarding [secure password hashing in PHP](http://stackoverflow.com/questions/401656/secure-hash-and-salt-for-php-passwords)
* You might work through this [PHP tutorial on securely hashing PHP passwords](http://net.tutsplus.com/tutorials/php/understanding-hash-functions-and-keeping-passwords-safe/)
* Check out the [portable PHP password hashing framework](http://www.openwall.com/phpass/)

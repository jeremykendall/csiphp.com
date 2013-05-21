---
author: jeremykendall
comments: true
date: 2011-07-15 07:15:43
layout: post
slug: permissions-are-hard
title: Permissions are hard
wordpress_id: 108
categories:
- Apache
---

[Justin Carmony](http://www.justincarmony.com/blog/) writes

> While not exactly in PHP, I found this in an old server's deployment script:
```
# Set Permissions
chmod -R 777 /var/
```

Because setting the most restrictive permissions that still allow your app to function is hard.

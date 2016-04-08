---
author: jeremykendall
comments: true
date: 2011-07-25 07:26:17
layout: post
slug: maslows-hammer
title: Maslow's hammer
wordpress_id: 130
categories:
- PHP
author: "Jeremy Kendall"
---
If all you have is PHP, everything looks like preprocessed hypertext.
``` php
<?php
print '
<HTML>
<HEAD>
<!-- Send users to the new location. -->
<TITLE>Acme Corp Redirect</TITLE>
<META HTTP-EQUIV="refresh" 
CONTENT="5;URL=http://anothersite.acmecorp.com"
</HEAD>
<BODY>
<img src="images/logo_acme.gif" alt="Acme Corp Logo"><br><br>
<strong>This page has moved. You will be 
automatically redirected 
to its new location in 5 seconds.
If not <a href="http://anothersite.acmecorp.com">click here</a>.</strong> 
</BODY>
</HTML>';
?>
```

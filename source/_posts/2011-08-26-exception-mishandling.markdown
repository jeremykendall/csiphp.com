---
author: jeremykendall
comments: true
date: 2011-08-26 08:18:17
layout: post
slug: exception-mishandling
title: Exception (mis)handling
wordpress_id: 261
categories:
- PHP
---

_Today's post was submitted by an anonymous CSI: PHP investigator._

There's some kind of design pattern going on here that I don't quite understand, I'm certain, because this is all over the place:

```php
<?php

try {
    // 20 - 100 lines of code...
} catch (Some_Application_Exception $e) {
    // TODO: Log Exception and re-raise...
} catch (Some_Framework_Exception $e) {
    // TODO: Log Exception and re-raise...
} catch (Exception $e) {
    // TODO: Log Exception and re-raise...
}
```

This isn't the worst. Apparently, simply saving a code snippet to paste in with all those application and framework exceptions was getting tedious (never mind the cost of maintaining them), so this (d)evolved eventually into:

```php
<?php

try {
    // 20 - 100 lines of code...
} catch (Exception $e) {
    // TODO: Log Exception and re-raise...
}
```

In the rare cases where the author was actually interested in the Exception thrown, we see lots of:
    
```php
<?php

try {
    // 20 - 100 lines of code...
} catch (Exception $e) {
    if ($e instanceof Some_Very_Specific_Application_Exception) {
        // Log Exception. That is all.
    }
}
```

The backlog is peppered with White Screen of Death (WSOD) errors that have no diagnosis and few steps to reproduce... And then there are the Ajax requests that seemingly fail for no reason. If this was just in legacy code that could be traced to (read: blamed on) one or more former employees, that would be one thing, but it appears to be cultural at this point.

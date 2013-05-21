---
author: jeremykendall
comments: true
date: 2011-09-30 10:04:00
layout: post
slug: the-cure-is-worse-than-the-disease
title: The cure is worse than the disease
wordpress_id: 356
categories:
- Education
- PHP
---

Oh, snap.

```php
<?php

function is_really_int(&$var) {
    $num = (int) $var;
    if ($var == $num) {
        $var = $num;
        return true;
    }
    return false;
}
```

This little bundle of joy really caught my attention. It's the _exact_ type of code that drove me to launch CSI: PHP.

**Context**

Apparently, the developer wanted `is_int(sqrt(16))` to return true. Since the return value of [`sqrt()`](http://us2.php.net/sqrt) is always a float, the dev wrote `is_really_int()` to return true whenever the return value of `sqrt()` could be accurately represented as an integer.

**The horror**

Fair enough. I even feel dude's pain. But the cure is worse, oh so much worse, than the disease.  Here are a few reasons why.
	
1. Userland method and function names should be descriptive.  I want to be able to know exactly (or have a good idea, at least) what's going to happen when I call your function.  If [`is_int`()](http://php.net/is_int) returns false, then why is asking, "Really?" going to change matters?
2. Passing by reference? Why? Functions prefaced by "is" should do one thing only: test something and return a boolean.  Changing the value of `$var` when the input can be an int violates this principle, is totally unexpected, and _will_ lead to unexpected behavior in your application.
3. This is the kind of code that you'll look at 6 months later and think, "Now what the hell does this do again?"  You'll want to shoot yourself in the face when you finally figure it out.
4. This is the kind of code that turns a maintenance developer into a [homicidal maniac](http://c2.com/cgi/wiki?CodeForTheMaintainer), and the maintainer is a bad mother-- (Shut your mouth!). I'm just talkin' about [funkatron](http://www.flickr.com/photos/jeremykendall/5762488063/)! (Then we can dig it)
5. The function returns wildly unexpected results (and [I can prove it](https://github.com/jeremykendall/int-or-not/blob/master/tests/IsReallyIntTest.php)).

**Something better?**

Again, I feel dude's pain.  If I get a float that can be represented as an integer, I'd like to be able to test for that without jumping through hoops.  I whipped this up to try and solve the problem without the issues caused by `is_really_int()`.
    
```php
<?php

/**
 * Tests if $var can be accurately represented as an integer.
 * Note that boolean values return false, just like is_int()
 * 
 * @param mixed $var Value to test
 * @return boolean 
 */
function valueCanBeInt($var)
{
    if (is_numeric($var) && $var == (int) $var) {
        return true;
    }

    return false;
}
```

It's true that I might wonder what the hell `valueCanBeInt()` does 6 months down the road, but a quick glance at the documentation and the function will make it clear.  I might even come up with a better name for the function at that point and do a little refactoring.  I'd be able to refactor with confidence, as I've written some . . .

**Unit tests**

Like I said, this one really captured my attention. [I've written some unit tests](https://github.com/jeremykendall/int-or-not/tree/master/tests) to demonstrate to myself and to you the dangerous side effects of `is_really_int()`. I ran the same tests against `valueCanBeInt`. The code and tests are [available on github](https://github.com/jeremykendall/int-or-not), if you're interested.

**UPDATE**: I totally forgot to thank [@Tyr43l](https://twitter.com/#!/Tyr43l/statuses/118670360869740544) for the submission.  Thanks Ferenc!

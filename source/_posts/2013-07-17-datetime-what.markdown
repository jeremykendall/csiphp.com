---
layout: post
title: "DateTime What?"
date: 2013-07-17 21:21
comments: true
categories: [PHP, "Rube Goldberg", SQL, RTFM]
---

I've seen a lot of crazy, tortured interactions between PHP and databases in my career, but [this particular solution](http://thedailywtf.com/Articles/PHP-Doesnt-Have-Date-Functions-Either.aspx) to the problem of displaying future dates is one of the most tortured I've ever seen.

The short story is that the developer in question _must_ have known SQL much better than any scripting language, chose to use SQL date math functions, queried the DB for persisted dates, and iterated over them in PHP.  Wat?

For the whole story, you'll need to head over to the always amazing The Daily WTF to read [PHP Doesn't Have Date Functions Either](http://thedailywtf.com/Articles/PHP-Doesnt-Have-Date-Functions-Either.aspx). You'll be glad you did.

Thanks to reader Michael Greiling [for the heads up](https://twitter.com/mikegreiling/status/357332903144853505) on the Daily WTF Post.

For more about PHP DateTime functionality, see the official [DateTime documentation](http://php.net/datetime) and the [Date and Time](http://www.phptherightway.com/#date_and_time) chapter at PHP: The Right Way.

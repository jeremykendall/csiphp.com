---
author: jeremykendall
comments: true
date: 2011-10-03 10:39:26
layout: post
slug: im-not-sure-i-can-think-of-a-worse-way-to-do-this
title: I'm not sure I can think of a worse way to do this
wordpress_id: 426
categories:
- PHP
author: "Jeremy Kendall"
---

If I think about it real hard, I might be able to find a worse way, but I don't think I'm that smart.

Behold, the awesome power of `xlate()`! (FYI, I added the documentation and refactored variable names for clarity)

```php
<?php

/**
 * Translates a string from one language to another.  If the requested translation
 * is not found in the database, inserts new translation to the database and returns
 * original string.
 * 
 * @param resource $connection Database connection resource
 * @param string $databaseName
 * @param string $string String to translate
 * @param string $languageFrom Two letter representation of language (en, sp, etc.)
 * @param string $languageTo Two letter representation of language (en, sp, etc.)
 * @return string Translated string if $string found in db, orignal $string if not
 */
function xlate($connection, $databaseName, $string, $languageFrom, $languageTo)
{
    $query = "SELECT $languageTo FROM xlate WHERE $languageFrom = '$string' ";
    $result = doQuery($connection, $databaseName, $query);
    $rows = mssql_num_rows($result);
    if ($rows > 0) {
        $row = mssql_fetch_assoc($result);
        $translation = $row[$languageTo];
        if ($translation == '') {
            return $string;
        } else {
            return $translation;
        }
    } else {
        $query = "INSERT INTO xlate($languageFrom) VALUES('$string')";
        $result = doQuery($connection, $databaseName, $query);
        return $string;
    }
}
```

I found this function because an app started timing out while building a form, exceeding the maximum execution time of 30 secs.  Turns out that for every single piece of text in that form, `xlate()` was being called, resulting in about 20 - 25 queries per page load.

A typical query looks like this.

```sql
SELECT en FROM xlate WHERE en = 'First Name *';
```

Considering the query is being called over and over in multiple loops, guess what that means?  Loops within loops and [queries within queries](http://csiphp.com/blog/2011/07/22/queries-inside-queries/).  FML.

The code is immediately suspect for another reason.  Notice how there's no key for the translation?  The English text (complete with the "required field ' *'") is used as the key, and coincidentally is exactly what's being returned.  That made me suspicious about the xlate table's schema.  Here it is.

```sql
-- SQL Server 2000
-- Translation table

CREATE TABLE [dbo].[db_xlate](
    [id] [bigint] IDENTITY(1,1) NOT NULL,
    [en] [nvarchar](100) NULL,
    [sp] [nvarchar](100) NULL,
    [fr] [nvarchar](100) NULL,
    [de] [nvarchar](100) NULL,
    [it] [nvarchar](100) NULL,
    [ru] [nvarchar](100) NULL,
    [pt] [nvarchar](100) NULL
) ON [PRIMARY]
``` 

Brilliant!  There's no message/translation key, so if you want an English translation, you call the string that's going to be returned anyhow in the where clause.  If you want to add a new language, you have to have to alter the schema to include a new column for language, and then add a translation for every single English item in the table (or risk null returns during translations).  If your translation isn't found, the function adds the translation, meaning a new row in the DB with a null value for all other language columns.  What's perhaps craziest is the fact that you have to know the translation in order to retrieve the translation, otherwise a wrong translation might be added to the DB, making the entire implementation null, void, and dangerous.

Like I said, I don't think I could do a worse job if I tried.

**Doing it better**

This one's easy; Translation is a solved problem, don't roll your own.  Did you hear me?  Do. Not. Roll. Your. Own.  You're wasting your time, you're making life harder on yourself, and you're making maintenance and continuing development near impossible for a maintenance programmer (or on the developer hired to take your place).  This is a perfect example of how dangerous [NIH](http://en.wikipedia.org/wiki/Not_Invented_Here) syndrome can be.

My experience with translation has been with [Zend Translate](http://framework.zend.com/manual/en/zend.translate.html), and I'm sure there are many other great options out there is you're not a Zend Framework person.  Pick something and run with it, and don't roll your own.

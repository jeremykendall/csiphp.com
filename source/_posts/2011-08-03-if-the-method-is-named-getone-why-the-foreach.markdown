---
author: jeremykendall
comments: true
date: 2011-08-03 07:15:10
layout: post
slug: if-the-method-is-named-getone-why-the-foreach
title: If the method is named getOne, why the foreach?
wordpress_id: 181
categories:
- PHP
---

I'll leave this to you to rip apart.  My head hurts too bad from banging it against the desk to do any real analysis, and it's not even my code.

```php
<?php
class Db
{
    // snip

    function getOneWithDetail($id=0)
    {
        $q = $this->createQuery('t')
            ->orderBy('t.title ASC')
            ->where('t.id = ?', $id);
        $objCollection = $q->execute();
        foreach ($objCollection as $objTitle) {
            return $objTitle; //there should be only one 
        }
    }

    // snip
}
``` 

Thanks to [Agustin Casiva](http://www.casivaagustin.com.ar/) for the submission.

---
author: jeremykendall
comments: true
date: 2011-09-07 10:23:57
layout: post
slug: no-lets-get-all-the-content-at-the-same-time
title: No, let's get ALL the content at the same time!
wordpress_id: 296
categories:
- PHP
author: "Jeremy Kendall"
---

And let's store it all in session!  What's more, if there's a database error, let's go ahead and let the users know about it with echo!  _Brilliant_!

```php
<?php
    
class BestCmsContentClassEvar
{
    protected function get_content()
    {
        $sql = 'select * from content order by id;';
        $result = $this->run_query($sql);
        //echo('looking for content');
        $i = 0;
        while ($row = mysql_fetch_array($result, MYSQL_ASSOC)) {
            if ($row['location'] == 'inc') {
                $_SESSION['content_name'][$i] = $row['name'];
                $_SESSION['content_title'][$i] = $row['title'];
                $_SESSION['content'][$i] = 'inc';
            } else {
                $_SESSION['content_name'][$i] = $row['name'];
                $_SESSION['content_title'][$i] = $row['title'];
                $_SESSION['content'][$i] = $row['content'];
            }
            $i++;
        }
        echo(mysql_error());
        mysql_close();
    }
}
```

**UPDATE**:  Thanks to [@Rob_OEM](http://twitter.com/rob_oem) for the image below.  It gave me the lulz.

{% img http://i.imgur.com/rM3BE.jpg %}

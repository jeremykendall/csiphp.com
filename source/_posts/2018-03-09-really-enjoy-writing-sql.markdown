---
layout: post
title: "Really enjoy writing SQL"
date: 2018-03-09 10:00
comments: true
categories: [ "PHP", "Sequel", "SQL" ]
author: "Joe Ferguson"
---

This great code snippet came from editor in chief of [php\[architect\]](https://www.phparch.com/) magazine, [Oscar Merida](https://twitter.com/omerida). The best step you can take to prevent your code from showing up here on our site is subscribe to [@phparch](https://twitter.com/phparch)

Oscar sent us this code with the statement "The user must have really enjoyed writing raw SQL statements"

``` php
<?php
if($something){
    if($venue_group_id){
        $query = "SELECT * FROM buildings WHERE building_group_id=$venue_group_id AND organization_id = $organization_id AND active = 1 ORDER BY venue_name ASC";
    }else{
        $query = "SELECT * FROM buildings WHERE active = 1 AND organization_id = $organization_id AND building_group_id = ".$session->building_group_id." ORDER BY venue_name ASC";
    }
}else{
    if($venue_group_id){
        $query = "SELECT * FROM buildings WHERE building_group_id=$venue_group_id AND active = 1 ORDER BY venue_name ASC";
    }else{
        $query = "SELECT * FROM buildings WHERE active = 1 AND building_group_id = ".$session->building_group_id." ORDER BY venue_name ASC";
    }
}
```

That's some interesting ordering of AND statements, let's clean it up a bit:

``` php
<?php
if($something){
    if($venue_group_id){
        $query = "SELECT * FROM buildings WHERE active = 1 AND organization_id = $organization_id AND building_group_id=$venue_group_id ORDER BY venue_name ASC";
    }else{
        $query = "SELECT * FROM buildings WHERE active = 1 AND organization_id = $organization_id AND building_group_id = ".$session->building_group_id." ORDER BY venue_name ASC";
    }
}else{
    if($venue_group_id){
        $query = "SELECT * FROM buildings WHERE active = 1 AND building_group_id=$venue_group_id ORDER BY venue_name ASC";
    }else{
        $query = "SELECT * FROM buildings WHERE active = 1 AND building_group_id = ".$session->building_group_id." ORDER BY venue_name ASC";
    }
}
```

Now we can more easily see what our four queries are doing here. If `$something` is true, then we search the `building_group_id` based on `$venue_group_id` unless it's false, then we use the value from `$session` instead. If `$something` is false, we again check the `$venue_group_id` and either use the value or again use the value from `$session`.

You're probably thinking "oh I could turn this into a 1 liner!", yes you likely could but I would caution you against over optimization and instead stick with code that is easier to human parse. Your future self will always appreciate your past self from being too clever.

If this was my code I would probably break up the sequel into parts and concatenate in the conditional statements so that we end up with the necessary query based on our conditions.

Happy Querying!
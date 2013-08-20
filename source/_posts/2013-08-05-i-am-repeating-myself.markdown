---
layout: post
title: "I Am Repeating Myself"
date: 2013-08-05 13:51
comments: true
categories: 
- PHP
- MySQL
- "SQL Injection"
---

> Today's entry is a guest post the from world-renowned [Michelangelo van Dam](https://twitter.com/DragonBe), PHP master, community hero, consultant, and all around nice guy.  Thanks for the post, Mike!

Yes, I'm repeating myself and apparently I need to. A quick search on github today revealed that 86,000+ people still use `$_GET` in their mysql statements!

{% tweet https://twitter.com/DragonBe/status/364342215998324736 %}

A few examples to get your attention:

```php
<?php

if (!is_numeric($_GET['id'])) {
    die("Numeric id !");
}
$id = $_GET['id'];
mysql_query("DELETE FROM executions WHERE executionId=$id");

switch($_GET['table']){
case "Sabores":
    $sql = "insert into Sabores values(0,'{$_GET['nombre']}',
        '{$_GET['descripcion']}','{$_GET['imagen']}','{$_GET['color']}')";

$getid= $_GET['id'];
mysql_query("DELETE FROM comment WHERE id='$getid'");
header("Location: success.php");
```

People please, **don't do it this way!** You're opening up your application to a whole lot of hurt and damage beyond repair if you implement this kind of code in production!

Remember this line: **Filter input, escape output**.

Filter all incoming data, no matter if they come from user input, database, web services or even from OCR using cameras!

{% img /images/tainted-data.png %}

Ensure this incoming data is always in the format you want and validates to rules you have defined. Yes, it's a lot of work to filter and validate everything, but a lot less to clean up your database when they've done a "Little Bobby Tables" on your application.

{% img http://imgs.xkcd.com/comics/exploits_of_a_mom.png %}

Use an abstraction layer like PDO and properly prepare statements with named variables to ensure data will be properly escaped before it enters your database. Even if you haven't taken the time to sanitise your data, this will be your final line of defence. Use it! Period!!!

This will be your basics to get started with PDO, but check out [php.net/pdo](php.net/pdo) for more information.

Getting a listing from MySQL:

```php
<?php
/**
 * Example how to list items from a databas using PDO
 *
 * @author Michelangelo van Dam
 */

ini_set('display_errors',1);
error_reporting(-1);

$pdo = new PDO(
    'mysql:host=localhost;dbname=<DBNAME>',
    '<USERNAME>',
    '<PASSWORD>'
);
$result = $pdo->query('SELECT * FROM `items`');
?>

<table>
    <tr>
        <th>ID</th>
        <th>ITEM</th>
        <th>PRICE</th>
    </tr>

    <?php while ($row = $result->fetch(PDO::FETCH_ASSOC)): ?>
    <tr>
        <td><?php echo htmlentities($row['id'], ENT_QUOTES, 'utf-8') ?></td>
        <td><?php echo htmlentities($row['item'], ENT_QUOTES, 'utf-8') ?></td>
        <td><?php echo htmlentities($row['price'], ENT_QUOTES, 'utf-8') ?></td>
    </tr>
    <?php endwhile ?>

</table>
```

Getting a single item from MySQL:

```php
<?php
/**
 * Example how to list items from a databas using PDO
 *
 * @author Michelangelo van Dam
 */
ini_set('display_errors',1);
error_reporting(-1);

// sanitize input and validate it's an integer
$id = filter_input(INPUT_GET, 'id', FILTER_VALIDATE_INT);

$pdo = new PDO('mysql:host=localhost;dbname=test', 'test', 'test');

// Use prepare statements for separating PHP and SQL
$stmt = $pdo->prepare('SELECT * FROM `items` WHERE `id` = :id');
$stmt->execute(array(':id' => $id));

// Get the result you want
$item = $stmt->fetch(PDO::FETCH_ASSOC);
?>

<dl>
    <dt><?php echo htmlentities($item['item'], ENT_QUOTES, 'utf-8') ?></dt>
    <dd><?php echo htmlentities($item['price'], ENT_QUOTES, 'utf-8') ?></dd>
</dl>
```

Now get moving to [php.net/pdo](php.net/pdo) and start learning doing things the right way!

-- Michelangelo van Dam

## Further Reading

If you'd like to dig into the topic of PHP security even further (and you do), check out these books that Mike recommends:

* [Essential PHP Security](http://amzn.com/059600656X) by Chris Shiflett
* [Pro PHP Security](http://amzn.com/1430233184)
* php|architect's [Guide to PHP Security](http://amzn.com/0973862106) by Ilia Alshanetsky

---
author: jeremykendall
comments: true
date: 2011-09-09 13:37:00
layout: post
slug: not-confusing-enough
title: Not confusing enough
wordpress_id: 308
categories:
- PHP
author: "Jeremy Kendall"
---

No, go ahead, add a little more abstraction.  Don't mind me, I'm just trying to maintain it is all.

```php
<?php

class core_output
{

    static public function factory($output_engine, $module)
    {
        switch ($output_engine) {

            case 'smarty':
                $file = OUT_SMARTY;
                break;
            default:

                $file = OUT_SMARTY;

                break;
        }

        if (include($file)) {
            $class = 'core_output_' . $output_engine;
            if (class_exists($class)) {
                $presenter = new $class($module);
                if ($presenter instanceof core_output_common) {
                    return $presenter;
                }

                echo 'Invalid presentation class: ' . $output_engine;
            }

            echo 'Presentation class not found: ' . $output_engine;
        }

        echo 'Presenter file not found: ' . $output_engine;
    }

//end factory
}
```

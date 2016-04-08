---
layout: post
title: "Everyone ALWAYS has JavaScript enabled right?"
date: 2016-04-08 14:36
comments: true
categories: [ "PHP", "PDO" ]
author: "Joe Ferguson"
---

A JavaScript redirect is a fairly simple technique to easily redirect your users to a new location. Within the confines of a PHP application if you need the parent window's URL, JavaScript is the easiest way to grab that info.


We have a PHP application that has a checkIfUserIsLoggedIn function and an $user_logged_in object is passed in to check if the user is logged in. On failure we need to redirect this user to the login screen on the same domain they're currently on. Keeping them on the same domain is something we have to do because this application responds to many different domain names.

Here is the original code

someIncludedFile.php
``` php
<?php
function checkIfUserIsLoggedIn($user_logged_in) {
	// Do something to check if login is active / current
	if (!$user_logged_in) {
	    // user inactive / session expired / user logged out
	    // redirect user to login screen on same domain
        ?>
        <script type="text/javascript">
            var pathToLoginScreen = '/path/to/login/screen
            var redirectURI = window.parent.location.href+pathToLoginScreen;
            window.location=redirectURI;
        </script>
        <?php
	}
}
```

somePartOfOurApp.php
``` php
<?php
include('someIncludedFile.php');
// this is our user login object used to check if login is active / current
$user_logged_in = $user->logged_in;

// Check if login is active/current
$is_user_logged_in = checkIfUserIsLoggedIn($user_logged_in);

// If the user's session is active / valid the app continues
// Otherwise the user should have been redirected via JavaScript
```

What is wrong with it?

* We are relying on the function to output raw JavaScript. This should be a check that gets acted on elsewhere.
* What happens if JavaScript is turned off?

It gets worse...

What happens when you do this type of log in check and then update a record?

``` php
<?php
include('someIncludedFile.php');

// this is our user login object used to check if login is active / current
$user_logged_in = $user->logged_in;

// Check if login is active/current
$is_user_logged_in = checkIfUserIsLoggedIn($user_logged_in);

// We didn't get redirected MUST be a good user RIGHT!?

// Do some active user logged in stuff
// get some data
$some_data = $data_source($find_this_data);
// update data from source
$update_result = $update_data($som_data);
```

There is still something wrong with it...

* If the user turns off JavaScript, the redirect never happens, and the code continues.
* Invalid users can update data as they please.

Where did we go wrong!?

* We could have avoided this by doing a hard exit after our JavaScript redirect in our PHP conditional.

someIncludedFile.php
``` php
<?php
function checkIfUserIsLoggedIn($user_logged_in) {
	// Do something to check if login is active / current
	if (!$user_logged_in) {
	    // user inactive / session expired / user logged out
	    // redirect user to login screen on same domain
        ?>
        <script type="text/javascript">
            var pathToLoginScreen = '/path/to/login/screen
            var redirectURI = window.parent.location.href+pathToLoginScreen;
            window.location=redirectURI;
        </script>
        <?php

        exit;
	}
}
```

Adding the exit after our JavaScript redirect would prevent any code below from being executed. Since we were not doing an exit previously, all the code under our JavaScript redirect would run if the user had JavaScript disabled in their browser. This also applies to browsers that do not support JavaScript.

Lessons learned: 

* Always exit your PHP if you have to do a non-PHP URI redirect!
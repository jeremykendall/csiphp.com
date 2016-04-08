---
layout: post
title: "Shortcuts Are Bad"
date: 2016-04-08 14:34
comments: true
categories: [ "PHP", "PDO" ]
author: "Joe Ferguson"
---

Friend of mine asked me to take a look at his PHP script. The original scope was to call this script via ajax, posting an email address to get saved to a database. 

Here is the original code

``` php
<?php
//email signup ajax call
	
	$MySQLConnectFile = 'congifdb.php';


if ( file_exists ( $MySQLConnectFile ) )
{
    $MySQLFileExists == TRUE;
}

if ( $MySQLFileExists )
{
    require_once ( $MySQLConnectFile );
}
else
{
    echo '<b>Error:</b> <i>Could not read the MySQL Connection File. Please try again later.';
	mysql_close();
    exit();
}

	
	//sanitize data
	$email = mysql_real_escape_string($_POST['signup-email']);
	
	//validate email address - check if input was empty
	if(empty($email)){
		$status = "error";
		$message = "You did not enter an email address!";
	}
	else if(!preg_match('/^[^\W][a-zA-Z0-9_]+(\.[a-zA-Z0-9_]+)*\@[a-zA-Z0-9_]+(\.[a-zA-Z0-9_]+)*\.[a-zA-Z]{2,4}$/', $email)){ //validate email address - check if is a valid email address
			$status = "error";
			$message = "You have entered an invalid email address!";
	}
	else {
		$existingSignup = mysql_query("SELECT * FROM signups WHERE signup_email_address='$email'");   
		if(mysql_num_rows($existingSignup) < 1){
			
			$date = date('Y-m-d');
			$time = date('H:i:s');
			
			$insertSignup = mysql_query("INSERT INTO signups (signup_email_address, signup_date, signup_time) VALUES ('$email','$date','$time')");
			if($insertSignup){ //if insert is successful
				$status = "success";
				$message = "You have been signed up!";	
			}
			else { //if insert fails
				$status = "error";
				$message = "Ooops, Theres been a technical error!";	
			}
		}
		else { //if already signed up
			$status = "error";
			$message = "This email address has already been registered!";
		}
	}
	
	//return json response
	$data = array(
		'status' => $status,
		'message' => $message
	);
	
	echo json_encode($data);
	mysql_close();
	exit;

?>
```

I immediately said "hey, let me take this and rework it a bit for you".

A few problems right off the bat:

* Not using PDO
* Not using filter_var()
* Not always outputting json
* Not using prepared statements


My first task was to move everything to use PDO and prepared statements. The second step was to break the "check if email already signed up" and "add email" functionality to their own functions. This makes the code a bit cleaner in my opinion. I also made sure every error or condition checked out put to the $status and $message variables so the ajax call would always return any error status.

dbconfig.php

``` php
<?php
$database_host = 'localhost';
$database_name = 'signup';
$database_user = 'user';
$database_password = 'I<3CSIPHP';
$conn = new PDO("mysql:host=$database_host;dbname=$database_name", $database_user, $database_password);
 
function CheckThisSignUp($email,$conn){
    $params = array(':email' => $email);
    $query = "SELECT * FROM signups WHERE signup_email_address = :email";
    $rows = $conn->prepare($query);
    $rows->execute($params);
    $data = $rows->fetchall();
    // close the database connection
    $errors = $conn->errorInfo();
    return $data;
}
function AddSignUp($email,$conn){
    $params = array(':email' => $email,':date'=>date('Y-m-d'),':time'=>date('H:i:s'));
    $query = "INSERT INTO signups (signup_email_address, signup_date, signup_time) VALUES (:email, :date, :time)";
    $create_user = $conn->prepare($query);
    $create_user->execute($params);
    if ($create_user->rowCount() == 1) { //if insert is successful
        return array('status'=>'success','message'=>'You have been signed up!');
    }
    else { //if insert fails
        return array('status'=>'error','message'=>'Ooops, Theres been a technical error!');
    }
}
```

Better than original code:

``` php
<?php
//email signup ajax call
$MySQLConnectFile = 'dbconfig.php';
 if (file_exists($MySQLConnectFile)){
    require_once ( $MySQLConnectFile );
} else {
    $status = "error";
    $message = "Could not read the MySQL Connection File. Please try again later!";
}
//sanitize data
$email = filter_var($_POST['signup-email'], FILTER_SANITIZE_EMAIL);
//validate email address - check if input was empty
if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
    $signup_check = CheckThisSignUp($email,$conn);
     if(empty($signup_check['0'])){
        $signup_results = AddSignUp($email,$conn);
        $status = $signup_results['status'];
        $message = $signup_results['message'];
    } else { //if already signed up
        $status = "error";
        $message = "This email address has already been registered!";
    }
} else {
	$status = "error";
    $message = "You have entered an invalid email address!";
}
//return json response
$data = array('status' => $status,'message' => $message);
echo json_encode($data);
?>
```

Lessons learned: 

* Use PDO over mysql_connect
* Be consistent in your error handling
* Prepared statements help keep your database safe.
* FILTER_SANITIZE_EMAIL & FILTER_VALIDATE_EMAIL Are very easy to use
* Friends don't let friends run bad code.
# Data Types and More

### Variables in PHP

```php
$name = 'Biyun'; // String
$number = 100; //int
$Num = 100.5; // float
$html = "<h1>hello</h1>"
```

### Concatenation

```php
echo $name . " " . $number;
// Biyun 100
```

### Math

```php
$number1 = 12;
$number2 = 24;

echo $number1 * $number2;

echo 56 + 45;
echo "<br>";
echo 56 - 45;
echo "<br>";
echo 56 * 45;
echo "<br>";
echo 56 / 45;   
echo "<br>"; 
```

### Arrays

```php
$numberList = array();
$numberList = [23, 23, '3', 4, '<h1>Hello</h1>']; // newer

echo $numberList[0];
print_r($numberList);
/*
23Array ( [0] => 23 [1] => 23 [2] => 3 [3] => 4 [4] =>
Hello
)
*/
```

### Associative Arrays

```php
$numbers = array('juanita', 'maria', 'jose');
print_r($numbers);

// associative
$names = array("first_name" => 'Biyun');
print_r($names);

// Array ( [0] => juanita [1] => maria [2] => jose ) 
// Array ( [first_name] => Biyun )  
```

# Control Structures

### if statements

```php
if(3 > 10) {
echo "three is more than ten";
} elseif(4 > 5) {
echo "of course four is less than five";
} else {
echo "it is not";
}
```

### Comparison and Logical Operators

equal ==

identical ===

compare >,  <,   >=,   <=,   <>

not equal !=

not identical !==

And &&

Or ||

Not !

### Switch Statements in PHP

```php
$number = 10;

switch($number) {

    case 34:
      echo "is it 34";
      break;
    case 37:
      echo "is it 37";
      break;
    case 35:
      echo "is it 35";
      break;
    case 24:
      echo "is it 24";
      break;
    default:
      echo "we could not find anything";
}
```

### While Loop in PHP

```php
$counter = 0;
while($counter < 10) {
  echo "hello student" . "<br>";
  $counter++;
}
```

### For Loop in PHP

```php
for($counter = 0; $counter < 10; $counter++) {
		echo "hello student" . "<br>";
}
```

### Foreach Loop in PHP

```php
$numbers = array(345,343,676,254,3657,5784);

foreach($numbers as $number) {
	echo $number . '<br>';
}
```

# Custom Functions

```php
function addNumbers($number1, $number2){

    $sum = $number1 + $number2;

    return $sum;
}

$res = addNumbers(345,3462);
```

### Global Variable and Scope

```php
$x = "outside"; // global

function convert() {
  $x = "inside"; // local 
}

echo $x;
echo "<br>";
convert();
echo $x;

// outside
// outside
```

```php
$x = "outside"; // global

function convert() {
  global $x;
  $x = "inside"; // local 
}

echo $x;
echo "<br>";
convert();
echo $x;

// outside
// inside
```

### Constants

```php
define('NAME', 1000);
echo NAME;

// or

const res = 14;
```

# How to use Form Data in PHP

form.php

```php+HTML
<?php 
	if (isset($_POST['submit'])) { // checking for form submission
		// extracting information from form
    $username = $_POST['username'];
    $password = $_POST['password'];
    
    // validating the form value
    $minimum = 5;

		if(strlen($username) < $minimum) {
			echo "Username has to be longer than " . $minimum;
		}
	}
?>

<form action="form.php" method="POST">
	<input type="text" placeholder="Enter Username">
	<input type="password" placeholder="Enter Password">
	<br>
	<input type="submit" name="submit">
</form>
```

# How to Use Database in PHP

### Connecting to the Database using PHP

db.php

```php
<?php
$servername = '127.0.0.1';
$user = 'root';
$pwd = '123456'; 
$database = 'loginapp';

$connection = mysqli_connect($servername, $user, $pwd, $database);
if($connection) {
  echo "We are connected";
} else {
  die("Database connection failed");
}
?>
```

### Creating Records into the database table with PHP

```php
$query = "INSERT INTO users(username, password) ";
$query .= "VALUES ('$username', '$password')";

$result = mysqli_query($connection, $query);
if(!$result) {
  die('Query Failed');
}
```

### Reading Information in the Database with PHP

```php+HTML
<?php
$query = "SELECT * FROM users";

$result = mysqli_query($connection, $query);
if(!$result) {
  die('Query Failed');
}
?>

<html>
<?php
    while($row = mysqli_fetch_assoc($result)) {
?>
    <pre>
       <?php
        print_r($row);
       ?>
    </pre>
<?php
   }
?>
</html>

<!--
				Array
(
    [id] => 1
    [username] => biyun
    [password] => superman
)
-->
```

### Refactoring the Update Query into a Function

functions.php

```php
<?php include "db.php"; ?>
<?php

function updateUser() {
  global $connection;
  $username = $_POST['username'];
  $password = $_POST['password'];
  $id = $_POST['id'];

  $query = "UPDATE users SET ";
  $query .= "username = '$username', ";
  $query .= "password = '$password' ";
  $query .= "WHERE id = '$id' ";

  $result = mysqli_query($connection, $query);
  if(!$result) {
    die("Query Failed" . mysqli_error($connection));
  }
}
?>
```

login_update.php

```php
<?php include "db.php"; ?>
<?php include "functions.php"; ?>
<?php
  if(isset($_POST['submit'])) {
    updateUser();
  }
?>
```

# PHP Security

### SQL Injection

$text = 'I'm a "foobar"';
mysqli_real_escape_string($text)  => `I\'m a \"foobar\"`

```php
  $username = $_POST['username'];
  $password = $_POST['password'];

  $username = mysqli_real_escape_string($connection, $username);
  $password = mysqli_real_escape_string($connection, $password);
```

### Password Encryption

```php
$hashed_password = password_hash($password, PASSWORD_DEFAULT);
```

# PHP and The Web

### Using the GET super global

```php
<?php
  print_r($_GET);
?>

// http://localhost/www/get.php?id=20 => Array ( [id] => 20 )
```

### Using the POST super global

```php+HTML
<?php 
	if (isset($_POST['submit'])) { 
    echo $_POST['name'];
	}
?>

<form action="form.php" method="POST">
	<input type="text" name="name">
	<input type="submit" name="submit">
</form>
```

### Setting Cookies with PHP

The information of the cookie of the user is saved in a super global variable `$_COOKIE`.

```php
$name = "SomeName";
$value = 100;
$expiration = time() + (60 * 60 * 24 * 7); // expire in 7 days

setcookie($name, $value, $expiration);
```

### Reading Cookies in PHP

```php
if(isset($_COOKIE['SomeName'])) {
    echo $_COOKIE['SomeName'];
}
// 100
```

### Session

```php
<?php session_start();

$_SESSION['greeting'] = "Hello Student this is awesome";

?>
```

# Object Oriented PHP

### Define a class

``` php
class Car {
  
}
if(class_exists("Car")) {
  echo "yeahhhhh";
}
```

### Adding methods and properties

```php
class Car {
  var $wheels = 4;
  var $hood = 1;
  var $engine = 1;
  var $doors = 4;
  
  function MoveWheels() {
    echo "wheel moves";
  }
  function AddWheels() {
    $this->wheels ++;
  }
}

if(method_exists("Car", "MoveWheels")) {
  echo "yeeees";
}
```

### How to instantiate a class

```php
$bmw = new Car();
$bmw->MoveWheels();
echo $bmw->wheels;
```

### Class inheritance

```php
class Plane extends Car {
  var $wheels = 20;
}

$jet = new Plane();
echo $jet->wheels; // 20
echo $jet->doors; // 4
$jet->MoveWheels();
```

### Constructor

A constructor is going to execute every time we create a new instance.

```php
class Car {

  var $wheels = 4;

  function __construct() {

    echo $this->wheels;
  }

}
$bmw = new Car(); // 4
```

### Data Access

```php

class Car {

  public $wheels = 4; // same as "var", available through whole program
  protected $hood = 1; // only available inside this class or any subclasses
  private $engine = 1; // only available inside this class
  var $doors = 4;

  function showProperty() {
    echo $this->hood;
  }

}
class Semi extends Car {
  function showEngine() {
    echo $this->engine;
  }
}

$bmw = new Car();
$semi = new Semi();
// protected
$bmw->showProperty(); // 1
echo $bmw->hood; // not working
$semi->showProperty(); // 1
// private
$semi->showEngine(); // not working
```

### Static Data in Classes

static properties are attached to the class, not to the instance.

```php
class Car {

  static $wheels = 4;
  var $doors = 4;

  function showProperty() {
    Car::$wheels = 10;
  }

}

$bmw = new Car();
echo $bmw->wheels; // not working
$bmw->showProperty();
echo Car::$wheels; // 10
```

# Working with files

### Opening and Creating files

```php
<?php

  $file = "example.txt";
  $handle = fopen($file, 'w');
  fclose($handle);
  
?>
```

### Writing to files

```php
  $file = "example.txt";
  
  if($handle = fopen($file, 'w')) {
    fwrite($handle, 'I love PHP');

    fclose($handle);
  } else {
    echo "The application was not able to write on the file";
  }
```

### Reading files

```php
  $file = "example.txt";

  if($handle = fopen($file, 'r')) {
    echo $content = fread($handle, filesize($file)); 

    fclose($handle);
  } else {
    echo "The application was not able to write on the file";
  }
```

### Deleting files

```php
<?php

  unlink("example.txt");

?>
```

# CMS Projet

### Display PHP variable in HTML

```php
echo "<li><a href='#'>{$cat_title}</a></li>";
```

### Display Data Dynamically

```php+HTML
<?php
  $query = 'SELECT * FROM posts';
  $select_all_posts = mysqli_query($conn, $query);

  while($row = mysqli_fetch_assoc($select_all_posts)) {
    $post_title = $row['post_title'];
    $post_author = $row['post_author'];
    $post_date = $row['post_date'];
    $post_image = $row['post_image'];
    $post_content = $row['post_content'];
?>

  <h2>    
    <a href="#"><?php echo $post_title; ?></a>
  </h2>
  <p class="lead">
    by <a href="index.php"><?php echo $post_author; ?></a>
  </p>
  <p><span class="glyphicon glyphicon-time"></span> <?php echo $post_date; ?></p>
  <hr>
  <img class="img-responsive" src="http://placehold.it/900x300" alt="">
  <hr>
  <p><?php echo $post_content; ?></p>
  <a class="btn btn-primary" href="#">Read More <span class="glyphicon glyphicon-chevron-right"></span></a>

  <hr>
<?php } ?>
```

### Insert the post image and display it

```sql
update posts
set post_image = 'image_1.jpg'
where post_id = 1 ;
```

```php+HTML
<img class="img-responsive" src="images/<?php echo $post_image ?>" alt="">
```

### Form

- use `multipart/form-data` when your form includes any `<input type="file">` elements
- otherwise you can use `multipart/form-data` or `application/x-www-form-urlencoded`but `application/x-www-form-urlencoded` will be more efficient

### Upload Image to images folder

```php+HTML
<div class="form-group">
  <label for="post_image">Post Image</label>
  <input type="file" name="post_image">
</div>

<?php
  if(isset($_POST['create_post'])) {
    $post_image = $_FILES['post_image']['name'];
    $post_image_temp = $_FILES['post_image']['tmp_name'];

    move_uploaded_file($post_image_temp, "../images/$post_image");
  }
?>
```

### Login and Session

​		use session to store the information of connected user

### Count rows from a table

```php
$query = "SELECT * FROM posts";
$select_all_posts = mysqli_query($conn, $query);
$posts_count = mysqli_num_rows($select_all_posts);
```

### Adding a Chart

```php+HTML
 <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
 <script type="text/javascript">
   google.charts.load('current', {'packages':['bar']});
   google.charts.setOnLoadCallback(drawChart);

    function drawChart() {
      var data = google.visualization.arrayToDataTable([
        ['Data', 'Count'],
        <?php
          echo "
          ['Posts', $posts_count],
          ['Comments', $comments_count],
          ['Users', $users_count],
          ['Categories', $categories_count]
          ";
        ?>
    	]);

    var options = {
      chart: {
        title: '',
        subtitle: '',
      }
    };

    var chart = new google.charts.Bar(document.getElementById('columnchart_material'));

    chart.draw(data, google.charts.Bar.convertOptions(options));
   }
 </script>
 <div id="columnchart_material" style="width: auto; height: 500px;"></div>
```

### Adding the WYSIWYG editor

![](https://tva1.sinaimg.cn/large/008i3skNly1guh566glrgj60l0052wen02.jpg)

```html
<!-- include libraries(jQuery, bootstrap) -->
<link href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" rel="stylesheet">
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>

<!-- include summernote css/js -->
<link href="https://cdn.jsdelivr.net/npm/summernote@0.8.18/dist/summernote.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/summernote@0.8.18/dist/summernote.min.js"></script>
```

```js
$(document).ready(function () {
  $('#summernote').summernote({
    height: 200,
  });
});
```

```html
<textarea class="form-control" name="post_content" id="summernote" cols="30" rows="10"></textarea>
```

### Adding Bulk Options 批量操作

thead

```html
<th><input id="selectAllBoxes" type="checkbox"></th>
```

tbody

```html
<td><input class='checkBoxes' type='checkbox' name='checkBoxArray[]' value='$post_id'></td>
```

```js
$(document).ready(function () {
  $('#selectAllBoxes').click(function (event) {
    if (this.checked) {
      $('.checkBoxes').each(function () {
        this.checked = true;
      });
    } else {
      $('.checkBoxes').each(function () {
        this.checked = false;
      });
    }
  });
});
```

### Return the ID generated in the last query

```php
mysqli_insert_id($conn); 
```

### PHP and Javascript Confirm before action

```php+HTML
<a onClick=\"javascript: return confirm('Are you sure you want to delete?');\" href='./posts.php?delete=$post_id'>Delete</a>
```

### Escape Strings

```php
function escape($string) {
  global $conn;
  return mysqli_real_escape_string($conn, trim($string));
}
```



# Documentation

### PHP 标记

- `<?php echo` 简写为 `<?=`
- 如果只有php代码，最好删掉`?>` 结束标记，避免输出空格或空行

# PHP framework

![php10](img/php10.jpeg)Folders

![php11](img/php11.jpeg)

### `public` folder

1. The only folder accessible to the web
2. The root of the web server, i.e. the folder http://localhost/ points to
3. The front controller and any static files go in here

Configuration in Apache: DocumentRoot => /framework/public

### Using a front controller

1. The URL doesn't map to an individual PHP script
2. All request are sent through one page
3. Handle everything common to every request, such as session handling

### Configure the web server to have pretty URLs in .htaccess

```
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?$1 [L,QSA]
```

### require or include

If the file is not found, `require` will stop the script and produce an error. `include` will just carry on.

### Create Router class

The router contains a table that matches routes to controllers and actions.

![php12](img/php12.jpeg)



### A regular expression for a simple URL structure

![php13](img/php13.jpeg)

The 'P' after '?' can be ignored 

![php14](img/php14.jpeg)



```php
$route = preg_replace('/\//', '\/', $route);
$route = preg_replace('/\{([a-z]+)\}/', '(?<\1>[a-z-]+)', $route);
$route = preg_replace('/\{([a-z]+):([^\}]+)\}/', '(?<\1>\2)', $route);
$route = '/^' . $route . '$/i';
```

### Controller

1. Controllers are what the user interacts with
2. They receive a request from the user, decide what to do, and send a response back.

### Controllers and actions

1. Controllers are `classes`
2. They contain `methods` that are the actions

#### Organise classes by using namespace

![php15](img/php15.jpeg)

![php16](img/php16.jpeg)

![php17](img/php17.jpeg)

![php18](img/php18.jpeg)

#### Class autoloading

![php19](img/php19.jpeg)

![php20](img/php20.jpeg)

#### The __call method

![php21](img/php21.jpeg)

#### Action filters: call a method before and after every action

![php22](img/php22.jpeg)



### View

1. Views are what the user sees on the screen
2. They present data to the user
3. just shows data, so contains a minimum amount of PHP: `echo, if, for` etc
4. Has no knowledge of models, sessions, databases etc.

### Controllers and Views

1. The controller doesn't write any output(i.e. HTML)
2. It loads and outputs a view file, which is what contains the content (HTML, JSON, XML etc.)

#### Output escaping

`htmlspecialchars()` get rid of cross-site attack

#### Pass data from the controller to the view

1. Extracting variables from an array

![php23](img/php23.jpeg)

#### Template Engine

1. Tool that helps to separate application code from presentation code
2. Templates contain no PHP at all: just HTML and simple tags to show data

![php24](img/php24.jpeg)

### Composer

#### Autoloader

```php
// composer.json
"autoload": {
    "psr-4": {
      "Core\\": "Core/",
      "App\\": "App/"
    }
  }
```

```
composer dump-autoload
```

### Model

1. Models are where an application's data stored
2. Responsible for storing and retrieving data
3. Models commonly store data in a database
4. A single model often has an equivalent database table, e.g. a `Post` model -> `posts` table

### PDO

(PHP Data Objects) is a code library for accessing databases.

![php25](img/php25.jpeg)

# Regular Expression

1. Metacharacters

![php26](img/php26.jpeg)

2. The start and end of the string

   ![php27](img/php27.jpeg)

3. Repetition

   ![php28](img/php28.jpeg)

4. Wildcards

   ![php29](img/php29.jpeg)

5. Escaping

   ![php30](img/php30.jpeg)

   

6. Character sets

   ![php31](img/php31.jpeg)

7. Character ranges

   ![php32](img/php32.jpeg)

8. Negated character sets

   ![php33](img/php33.jpeg)

9. Capture groups

   ![php34](img/php34.jpeg)

10. Named capture groups

    ![php35](img/php35.jpeg)

11. Backreferences to capture groups

    ![php36](img/php36.jpeg)

### Regular expression matching

![php37](img/php37.jpeg)

![php38](img/php38.jpeg)

![php39](img/php39.jpeg)

![php40](img/php40.jpeg)



### Replace parts of strings using regular expressions

![php41](img/php41.jpeg)

![php42](img/php42.jpeg)

![php43](img/php43.jpeg)
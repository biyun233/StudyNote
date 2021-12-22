# PHP framework

![](https://tva1.sinaimg.cn/large/008i3skNgy1gv7txo8bo1j60l90a0aag02.jpg)

### Folders

![](https://tva1.sinaimg.cn/large/008i3skNgy1gv7ujg3nelj60by0b8q3202.jpg)



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

![](https://tva1.sinaimg.cn/large/008i3skNly1gv7zulg7nfj60q5083wfb02.jpg)



### A regular expression for a simple URL structure

![](https://tva1.sinaimg.cn/large/008i3skNly1gv7zxv08f9j60oi0b475002.jpg)

The 'P' after '?' can be ignored 



![](https://tva1.sinaimg.cn/large/008i3skNgy1gvb6784r5zj60r90adjrz02.jpg)

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

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvb8i9vmnpj60qe0ac74z02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvb8j7v3d6j60md0abgm602.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvbac8meogj60qd09swf202.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvbachx6jxj60ql0btab402.jpg)

#### Class autoloading

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvbainfngcj60pe0ardgn02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvbalqme3gj60pp08ot9h02.jpg)

#### The __call method

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvbcuairavj60qm0ak3zn02.jpg)

#### Action filters: call a method before and after every action

![](https://tva1.sinaimg.cn/large/008i3skNly1gvbd0r6esij60r20bngmo02.jpg)



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

1. Extracting variables from an array<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvbfj45edaj60n30ba3z602.jpg" style="zoom:80%;" />



#### Template Engine

1. Tool that helps to separate application code from presentation code
2. Templates contain no PHP at all: just HTML and simple tags to show data

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvbfp37gj8j60oz04jt9b02.jpg" style="zoom:80%;" />

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

![](https://tva1.sinaimg.cn/large/008i3skNly1gvca02uvlaj60nk03kq3c02.jpg)

# Regular Expression

1. Metacharacters

![](https://tva1.sinaimg.cn/large/008i3skNgy1gv7wd61tt4j60fj04cq3702.jpg)

2. The start and end of the string

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7yy5xjuij60a302mjrb02.jpg)

3. Repetition

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7z2sn9nrj607z02hq2s02.jpg)

4. Wildcards

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7z3djxc3j60m401m74802.jpg)

5. Escaping

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7z3x8qjhj60gi01o3yf02.jpg)

6. Character sets

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7z4oz25xj60on025dfy02.jpg)

7. Character ranges

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7z595zawj60pb02eaa902.jpg)

8. Negated character sets

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7z66q1khj60px02bq3302.jpg)

9. Capture groups

   ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7zlqupi6j60ps02aglq02.jpg)

10. Named capture groups

    ![](https://tva1.sinaimg.cn/large/008i3skNly1gv7znq1spuj60ix01oaa002.jpg)

11. Backreferences to capture groups

    ![](https://tva1.sinaimg.cn/large/008i3skNly1gv80g5rcv4j60l801hq2w02.jpg)

### Regular expression matching

![](https://tva1.sinaimg.cn/large/008i3skNly1gv7zk20kppj60hg08hwes02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gv7zkdpz5xj60je08yjrr02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gv80hlk9nwj612607imxl02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gv7zorkv0oj60qi06q0t202.jpg)



### Replace parts of strings using regular expressions

![](https://tva1.sinaimg.cn/large/008i3skNly1gv80c9rebtj60pd07ct9702.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gv80d3vukpj60pb02zweh02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gv80hzr39ej616405wwey02.jpg)
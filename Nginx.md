## NGINX



### What happens when you type 'www.google.com' in your browser and press Enter

- Your computer sends a request to the domain name system (DNS) server which serves as an address book for all domain names. This then sends back the exact IP address of the server which https://www.google.com points to
- Knowing this IP, your computer then establishes a connection with the server through the IP address. The type of this connection is known as Transmission Control Protocol (TCP) and your computer is able to establish this connection through the Internet Protocol (IP). This whole process is known as a "handshake"
- If your computer is behind a firewall, the firewall checks to ensure that the particular request you are making is allowed before permitting it. Also, if the server you are trying to access is also behind a firewall, a similar check will be done before you are finally able to connect to the server.
- After establishing the connection, your browser now sends a request for the webpage using an encryption protocol like Secure Sockets Layer (SSL) or Transport Layer Security (TLS) in order to encrypt the data that will be shared between your computer and the server. This type of encryption is what is responsible for the "s" in "https" which also implies that the connection is secure.
- Companies like Google with high traffic maintain a host of servers and for that matter they have a load balancer that receives most of the requests and sends it to a particular server. The request from your browser will therefore hit the load balancer first which will forward it to a specific server depending on the algorithm used by the load balancer.
- The server that receives the request then sends a response back to the load balancer which also forwards the response back to your browser. This response will mostly include HTML, CSS and JavaScript files that makes up Google's homepage.
- Finally, the browser will render the page and display it to you.



### What is Nginx?

- An open source software
- Web server for reverse proxying, caching and load balancing
- Provides HTTP server capabilities
- Designed for maximum performance and stability
- Functions a proxy server for email (IMAP, POP3, SMTP)
- Uses a Non-threaded and event-driven architecture



### Nginx Architecture

Nginx Uses Master-Slave architecture by supporting event-driven, asynchronous and non-blocking model.

![nginx1](img/nginx1.png)

 

### Configuration Settings

The core settings of NGINX are mainly configured in the nginx.conf file. The configuration file mainly structured into Contexts.

- Worker_processes: defines the number of worker processes that nginx will use, usually be equal to the number of CPU cores
- Worker_connections: the maximum number of simultaneous connections for each worker_processes, and tells a worker_process how many people can simultaneously be served by nginx
- Access_log & error_log: the files that nginx will use to log any errors and access attempts, used for debugging and troubleshooting
- gzip: compression of nginx responses



### How to install Nginx?

```sh
sudo apt-get install nginx

# enable simple firewall
sudo ufw enable
sudo ufw app list
# Available applications:
#  Apache
#  Apache Full
#  Apache Secure
#  Nginx Full : port 80 and port 443
#  Nginx HTTP : port 80
#  Nginx HTTPS : port 443
#  OpenSSH
#  Postfix
#  Postfix SMTPS
#  Postfix Submission

# port 80: allows the unencrypted web traffic
# port 443: allows the encrypted traffic

sudo ufw allow 'Nginx Full'
sudo ufw status
sudo systemctl status nginx
```



### Controlling NGINX

To reload your configuration, you can stop or restart NGINX, or send signals to the master process. A signal can be sent by running the `nginx` command (invoking the NGINX executable) with the `-s` argument.

```sh
nginx -s <SIGNAL>
```

where `<SIGNAL>` can be one of the following:

- `quit` – Shut down gracefully (the `SIGQUIT` signal)
- `reload` – Reload the configuration file (the `SIGHUP` signal)
- `reopen` – Reopen log files (the `SIGUSR1` signal)
- `stop` – Shut down immediately (or fast shutdown, the `SIGTERM` singal)



### NGINX Logs

#### Access Logs

files that have the information of all the resources that a client is accessing on the NGINX server, such as details about what is being accessed and how it responded to the requests, including client IP address, response status code, user agent, and more.

```sh
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
			'$status $body_bytes_sent "$http_referer" '
			'"$http_user_agent" "$http_x_forwarded_for"';
access_log /var/log/nginx/access.log  main;

# 192.168.110.203 - - [21/Mar/2023:13:05:18 +0800] "GET /api/product/all/list?current_page=1&page_size=10 HTTP/1.1" 200 753 "http://npd.test.heymenology.cn/product-list" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36"
```

<u>NGINX Access Log Fields:</u>

- **remote_addr**: The IP address of the client that requested the resource
- **http_user_agent**: The user agent in use that sent the request
- **time_local**: The local time zone of the server
- **request**: What resource was requested by the client (an API path or any file)
- **status**: The status code of the response
- **body_bytes_sent**: The size of the response in bytes
- **request_time**: The total time spent processing the request
- **remote_user**: Information about the user making the request
- **http_referer**: The IP address of the HTTP referer
- **gzip_ratio**: The compression ratio of gzip, if gzip is enabled



```sh
# access_log path [format [buffer=size] [gzip[=level]] [flush=time] [if=condition]];

# format name defined by yourself
map $remote_addr $log_enable {
    "192.168.4.1" 0;
    "192.168.4.2" 0;
    "192.168.4.3" 0;
    "192.168.4.4" 0;
    default 1;
}
access_log /var/log/nginx/access.log combined if=$log_enable
```



#### Error Logs

files where all information about errors will be logged, including permission errors or any NGINX configuration-related access errors.

```sh
# error_log log_file_location log_level;
```

<u>NGINX Error Log Levels</u>

1. **emerg**: These are the emergency logs. They mean that the system is unusable.
2. **alert**: An immediate action is required.
3. **crit**: A critical condition occurred.
4. **error**: An error or failure occurred while processing a request.
5. **warn**: There was an unexpected event, or something needs to be fixed, but NGINX fulfilled the request as expected.
6. **notice**: Something normal, but important, has happened, and it needs to be noted.
7. **info**: These are messages that give you information about the process.
8. **debug**: These are messages that help with debugging and troubleshooting. They are generally not enabled unless needed because they create a lot of noise.

Note that the log_level parameter is a threshold, as every log level includes the previous log levels as well. For example, if your log level is 6 (notice), your logs will contain entries from levels 1 through 6.

<u>Logging to Multiple Files</u>

```sh
error_log /var/log/nginx/error.info info;
error_log /var/log/nginx/error.crit crit;
```



### Grammar

```sh
server {
	listen 80;
  server_name biyun233.com;
  index index.html;
  root /var/www/blog;
  
	location /files {
		# This is a standard location
		# Will be match /files or /filesxxx
	}
	
	# Preferential Regex Block #
	location ^~ /filesinfo {
		# This is an example like standard location, but more important than regex
		# Will be match /filesinfo or /filesinfoxxx
	}
	
	location ~ /files[0-9] {
    # regex case sensitive location
    # Will be match /files1xxx etc
  }

  location ~* /files[0-9] {
    # regex case insensitive location
    # Will be match /Files2xxx
  }
}
```

- `=`：精确匹配，用于不含正则表达式的 uri 前，如果匹配成功，不再进行后续的查找
- `^~`：前缀匹配，用于不含正则表达式的 uri 前，表示如果该符号后面的字符是最佳匹配。采用该规则，不再进行后续的正则查找。跟 `=` 的区别是，不需要 uri 一模一样，只需要开头和 uri 匹配即可。
- `~`：正则匹配，表示用该符号后面的正则 uri 去匹配路径，区分大小写
- `~*`：正则匹配，表示用该符号后面的正则 uri 去匹配路径，不区分大小写。
- `空`：普通匹配（最长字符匹配），匹配以 uri 开头的字符串，只能是普通字符串。例如，`location /` 是通用匹配，任何请求都会匹配到。另外普通匹配与 location 顺序无关，是按照匹配的长短来确定匹配结果。

#### 优先级

`location =` > `location 完整路径`（还会去匹配正则） > `location ^~ 路径` > `location ~,~* 正则顺序` > `location 部分起始路径` > `location /`

- 在所有匹配成功的 uri 中，选取匹配度最长的 uri 字符地址。正则除外，正则匹配是按照先后顺序确定匹配结果

- 正则匹配成功之后停止匹配，普通匹配成功后还会接着匹配正则。举个例子：

  ```sh
  # `location 完整路径` 虽然比 `location ^~ 路径` 优先级高，但还是会去匹配正则
  # 如果正则匹配成功，采用正则匹配结果。如果没有匹配到 `location 完整路径`，`location ^~ 路径` 就比 `location ~,~* 正则顺序` 优先级高了
  location ~ /ab {
    rewrite ^ http://baidu.com/s?word=A;
  }
  location /abc {
    rewrite ^ http://baidu.com/s?word=B;
  }
  location ^~ /ab {
    rewrite ^ http://baidu.com/s?word=C;
  }
  
  #访问 http://docs.chenfangxu.com/ab 会跳到百度搜索关键词 C
  #访问 http://docs.chenfangxu.com/abc 会跳到百度搜索关键词 A
  
  ```

  

- 如果 uri 包含正则表达式，则必须有 `~` 或 `~*` 标志，否则正则代码只能作为普通字符使用，例如 `location = /demo$` ，其中的 `$` 并不代表正则模式结束，而是一个实实在在的 `$` 字符，是 url 的一部分。
- 针对 `~` 和 `~*` 匹配标识符，可以在前面加上 `!` 来取反：`!~`：表示正则不匹配，区分大小写， `!~*`：表示正则不匹配，不区分大小写



#### root VS alias

```sh
location /files {
	root /var/www/content # final_path = /var/www/content/files
	alias /var/www/assets/files # final_path = /var/www/assets/files
}
```

### Directives 命令

#### **client_max_body_size**

```
Syntax:	client_max_body_size size;
Default:	client_max_body_size 1m;
Context:	http, server, location
```

Sets the maximum allowed size of the client request body. If the size in a request exceeds the configured value, the 413 (Request Entity Too Large) error is returned to the client.

Setting `size` to 0 disables checking of client request body size.

#### client_body_buffer_size

```
Syntax:	client_body_buffer_size size;
Default:	client_body_buffer_size 8k|16k;
Context:	http, server, location
```

Sets buffer size for reading client request body. In case the request body is larger than the buffer, the whole body or only its part is written to a temporary file.

#### **try_files**

```
Syntax:	try_files file ... uri;
				try_files file ... =code;
Default:	—
Context:	server, location
```

#### log_not_found

```
Syntax:	log_not_found on | off;
Default:	log_not_found on;
Context:	http, server, location
```

Enables or disables logging of errors about not found files into error_log.


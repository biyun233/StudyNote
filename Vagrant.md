# Vagrant

### Basic commands

```shell
vagrant init laravel/homestead
vagrant init bento/ubuntu-20.04
vagrant up
# 启动
vagrant ssh

vagrant destroy
vagrant halt

# 修改vagrant配置
vi Vagrantfile
vagrant reload
```

### 修改**vagrant**时区

```shell
tzselect
sudo cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 修改PHP时区

```shell
cd /etc/php/8.0/fpm  # fpm: 代码， cli: 命令行
sudo vi php.ini
# date.timezone = "Asia/Shanghai"
sudo systemctl restart php8.0-fpm
sudo systemctl restart nginx

```

### NGINX相关

```shell
#可理解为上线
/etc/nginx/sites-enabled    
#可理解为准备上线
/etc/nginx/sites-availble   

#将准备上线的配置 软链接到上线文件夹
sudo ln -s /etc/nginx/sites-available/supply-chain.conf /etc/nginx/sites-enabled/supply-chain.conf   

#取消上线，此时sites-available中还存有相关文件
sudo rm /etc/nginx/sites-enabled/supply-chain.conf  

```

### NGINX配置

test.conf

```shell
server {
    listen 80 default_server;
    server_name _;

    index index.php index.html;
    root /vagrant/nginx-test/;

    # 路由
    rewrite ^/shipment/(.*) /controller/shipment/$1.php last;
    rewrite ^/(.*) /controller/$1.php last;

    location ~ \.php {
        try_files $uri /index.php =404;
        #fastcgi_pass 127.0.0.1:9000;
        fastcgi_pass unix:/run/php/php8.0-fpm.sock;
        include fastcgi_params;
        fastcgi_index index.php;
        fastcgi_split_path_info ^(.+\.php)(.*)$;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

每次修改配置需重启nginx

```shell
sudo systemctl restart nginx
```

查看nginx状态

```sh
systemctl status nginx
```

查看nginx错误日志

```sh
cat /var/log/nginx/error.log
```



### PHP 部署

- ### 下载相关工具

  1. VMware ovftool (https://code.vmware.com/web/tool/4.4.0/ovf)

  2. ```shell
     vagrant plugin install vagrant-vmware-esxi
     ```

  ### OVFtool 可能出现Error

  - ![截屏2021-12-01 13.18.43](https://tva1.sinaimg.cn/large/008i3skNly1gwyadzv8yaj312m02874p.jpg)

    ```sh
    ln -s /Applications/VMware\ OVF\ Tool/ovftool /usr/local/bin
    ```

  - ![截屏2021-12-01 13.18.49](https://tva1.sinaimg.cn/large/008i3skNly1gwyag9zlpbj314a05st9q.jpg)

    ```shell
    cd /Users/Biyun/.vagrant.d/boxes/bento-VAGRANTSLASH-ubuntu-20.04/202110.25.0/vmware_desktop
    # modify ubuntu-20.04-amd64.vmx and ZZZZ_supply-chain.vmx
    ide0:0.devicetype = "cdrom-raw"
    ide0:0.filename = "auto detect"
    ```

- 新建虚拟机

  ```
  vagrant init bento/ubuntu-20.04
  ```

- Vagrantfile 所需配置

  ```shell
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.hostname = "supply-chain"
  config.vm.synced_folder("./", "/vagrant", disabled: true)
  #修改时区
  config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime", run: "always"
  config.vm.provider :vmware_esxi do |esxi|
      esxi.esxi_hostname = '192.168.10.10'
      esxi.esxi_username = 'root'
      esxi.esxi_password = 'HEYme@123!'
      esxi.guest_numvcpus = '1'
      esxi.guest_memsize = '2048'
  end
  ```
  
- ```sh
  vagrant up
  vagrant ssh
  ```

- install nginx, php, mysql

  ```sh
  sudo vi install.sh
  sudo chmod +x install.sh
  ./install.sh
  ```

  

  ```sh
  #!/usr/bin/env bash
  
  set -euxo pipefail
  
  sudo apt-get update
  sudo apt install -y nginx
  
  sudo sed -i 's/$nginx_version/0.0.0/g' /etc/nginx/fastcgi.conf
  sudo sed -i 's/# gzip/gzip/g' /etc/nginx/nginx.conf
  sudo sed -i 's/# server_tokens off/server_tokens off/g' /etc/nginx/nginx.conf
  sudo sed -i 's/access_log \/var\/log\/nginx\/access.log/access_log off/g' /etc/nginx/nginx.conf
  
  sudo rm -f /etc/nginx/sites-enabled/default
  
  sudo systemctl enable nginx
  
  sudo apt install -y \
  	php7.4-cli \
  	php7.4-common \
  	php7.4-curl \
  	php7.4-fpm \
  	php7.4-gd \
  	php7.4-json \
  	php7.4-mbstring \
  	php7.4-mysql \
  	php7.4-opcache \
  	php7.4-readline \
  	php7.4-xml \
  	php7.4-zip
  
  sudo sed -i "s/^;slowlog =/slowlog =/g" /etc/php/7.4/fpm/pool.d/www.conf
  sudo sed -i 's/^expose_php = On/expose_php = Off/g' /etc/php/7.4/fpm/php.ini
  
  sudo systemctl enable php7.4-fpm
  
  MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32)
  
  sudo apt update
  sudo apt install -y mysql-server
  sudo systemctl enable mysql
  sudo systemctl start mysql
  
  sudo apt install -y expect
  
  tee ~/secure_our_mysql.sh > /dev/null << EOF
  spawn $(which mysql_secure_installation)
  expect "Press y|Y for Yes, any other key for No:"
  send "y\r"
  expect "Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG:"
  send "2\r"
  expect "New password:"
  send "$MYSQL_ROOT_PASSWORD\r"
  expect "Re-enter new password:"
  send "$MYSQL_ROOT_PASSWORD\r"
  expect "Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) :"
  send "y\r"
  expect "Remove anonymous users? (Press y|Y for Yes, any other key for No) :"
  send "y\r"
  expect "Disallow root login remotely? (Press y|Y for Yes, any other key for No) :"
  send "y\r"
  expect "Remove test database and access to it? (Press y|Y for Yes, any other key for No) :"
  send "y\r"
  expect "Reload privilege tables now? (Press y|Y for Yes, any other key for No) :"
  send "y\r"
  EOF
  
  sudo expect ~/secure_our_mysql.sh
  
  rm -v ~/secure_our_mysql.sh
  
  MYSQL_DEBIAN_CONF=/etc/mysql/debian.cnf
  
  set +x
  echo "Enter the mysql user that you want to create: "
  read username
  
  echo "Enter the password: "
  read -s password
  
  echo "Enter the database that you want to create: "
  read database
  
  set -x
  
  sudo mysql --defaults-file=$MYSQL_DEBIAN_CONF -e "create user '$username'@'localhost' identified by '$password';"
  sudo mysql --defaults-file=$MYSQL_DEBIAN_CONF -e "create database $database;"
  sudo mysql --defaults-file=$MYSQL_DEBIAN_CONF -e "grant all on $database.* to '$username'@'localhost';"
  sudo mysql --defaults-file=$MYSQL_DEBIAN_CONF -e "flush privileges;"
  
  
  
  # 禁止访问3306
  sudo iptables -I INPUT -p TCP --dport 3306 -j DROP                                                                       
  # 允许本机访问
  sudo iptables -I INPUT -s 127.0.0.1 -p TCP --dport 3306 -j ACCEPT
  
  exit 0
  
  ```

  ```sh
  # check php, nginx, mysql installed
  # username: dev
  nginx -v
  php -v
  mysql -udev -p
  ```

  安装composer，npm

  ```sh
  sudo apt install composer
  # Using Ubuntu
  curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
  sudo apt-get install -y nodejs
  ```

- 为git生成ssh

  ```sh
  ssh-keygen -t rsa -b 4096
  cat .ssh/id_rsa.pub # 需把ssh加入git账号
  
  ```

- 克隆并转移项目

  ```sh
  # backend
  git clone (backend_project_addr)
  cd backend_project_addr
  composer install
  
  # frontend
  git clone (frontend_project_addr)
  cd frontend_project_addr
  npm install
  npm run build
  
  cd /var/www
  sudo mkdir supply-chain
  cd supply-chain
  # for backend
  sudo cp -r (backend_project_addr/folders_needed) .
  
  # for frontend
  sudo mkdir public
  sudo cp -r (frontend_project_addr/dist/*) .
  
  
  # 配置文件准备上线
  sudo rm /etc/nginx/sites-available/supply-chain.conf 
  sudo cp supply-chain.conf /etc/nginx/sites-available/
  sudo rm /etc/nginx/sites-enabled/supply-chain.conf 
  # 把配置文件软链接到上线文件夹
  sudo ln -s /etc/nginx/sites-available/supply-chain.conf /etc/nginx/sites-enabled/supply-chain.conf
  # 重启nginx
  sudo systemctl restart nginx
  ```

  
### Install tools basic Ubuntu22.04

```sh
sudo apt update
sudo apt install php8.1
sudo apt install php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-fpm php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-redis php8.1-intl -y
sudo apt install nginx
sudo systemctl enable nginx
sudo apt install mysql-server
sudo apt install composer
sudo apt install redis
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install supervisor
# Ubuntu22.04 不支持直接下载php7.4
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php7.4
sudo apt install php7.4-common php7.4-mysql php7.4-xml php7.4-json php7.4-mbstring php7.4-gd php7.4-curl

```

### create mysql user

### init redis

```sh
sudo vi /etc/redis/redis.conf
# port 6380
# requirepass password
```

### stop apache

```sh
sudo systemctl stop apache2
sudo systemctl disable apache2
sudo apt remove apache2
sudo apt autoremove

```


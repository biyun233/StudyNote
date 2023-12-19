# Trouble Shooting

### list users

```sh
cat /etc/passwd
userdel username # remove user
```



### Writing permission denied

```
cat /etc/group
```

- current user : voltage

- file owner : www-data

- file permission : -rw-rw-r--

- ```sh
  # add voltage to www-data group
  sudo adduser voltage www-data
  ```

### Install PHP 8.1 on ubuntu20.04

```sh
sudo apt update
sudo apt upgrade
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.1
sudo apt install php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-fpm php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-redis php8.1-intl -y
```

#### change PHP version

```sh
sudo update-alternatives --config php
```

#### 开机自启动

- Service

  - ```sh
    systemctl list-unit-files --type=service --state=enabled
    systemctl is-enabled redis-server
    systemctl enable redis-server
    ```

- 3rd party

  - ```sh
    # in crontab
    @reboot sh test.sh
    ```

    

### Redis

修改默认端口和密码

```sh
sudo vi /etc/redis/redis.conf
# port 6380
# requirepass password
```

```sh
redis-cli -p 6380 -a password
```



### sudo without password

```sh
sudo visudo
# voltage ALL=(ALL) NOPASSWD:ALL
```

### ubuntu 替换服务地址

```sh
cd /etc/apt
sudo tar -zcvf sources.list.tar.gz sources.list
sudo vi sources.list
# 将 cn.archive.ubuntu.com 替换为 mirrors.aliyun.com
```

### install Nodejs

```sh
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

### Install yarn

```sh
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/yarn.gpg
echo "deb [signed-by=/etc/apt/trusted.gpg.d/yarn.gpg] https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn
```

```sh
# 使用yarn install 报错：error An unexpected error occurred: "https://registry.yarnpkg.com/@heroicons%2fvue: ETIMEDOUT".
# 把资源地址设置成npm淘宝源
npm config set registry https://registry.npm.taobao.org
npm config set disturl https://npm.taobao.org/dist


```


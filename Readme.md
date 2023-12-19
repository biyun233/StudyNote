## 环境配置

#### 项目初始化

```sh
cd (path_to_file)/voltage_project
composer install
cp .env.example .env
# 根据需要配置 .env 文件
php artisan key:generate
php artisan storage:link
```

#### PHP 配置

```php
# php.ini文件需注意以下配置
memory_limit = 1024M
extension=fileinfo
extension=pdo_mysql
extension=gd
extension=php_imagick.dll
# 若没有imagick插件，需要下载
```

#### 项目启动

```
php artisan serve
```


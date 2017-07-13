# Laravel

### 从Docker容器安装Laravel

1.进入工作区容器

```
docker-compose exec --user=laradock workspace bash
```

2.安装Laravel

```
composer create-project laravel/laravel my-cool-app "5.4.*"
```

3.修改映射路径

默认情况下的映射目录在.env的APPLICATION设置的laravel的父目录中 , 可以修改.env配置 , 也可以修改yml文件

```
application:
         image: tianon/true
        volumes:
            - ../my-cool-app/:/var/www
    ...
```

> 这个目录绑定在workspace容器的/var/www下

### 工作区容器的其他命令行进入

```
# 确保有工作区运行
docker-compose up -d workspace
# 查看工作区
docker-compose ps
# 进入工作区
docker-compose exec --user=laradock workspace bash
# 可以运行很多命令
php artisan
composer update
phpunit
```

---

### 运行Laravel的队列

先添加php-worker容器 , 就像添加PHP-FPM容器一样 .

可以配置docker-compose.yml文件 :

```
php-worker:
  build:
    context: ./php-fpm
    dockerfile: Dockerfile-70 # or Dockerfile-56, choose your PHP-FPM container setting
  volumes_from:
    - applications
  command: php artisan queue:work
```

```
docker-compose up -d php-worker
```

### 使用Redis

```
docker-compose up -d redis
# 配置Laravel的.env文件
REDIS_HOST=redis
# 也可以在config/database.php配置
'redis' => [
    'cluster' => false,
    'default' => [
        'host'     => 'redis',
        'port'     => 6379,
        'database' => 0,
    ],
],
```

配置Laravel的Cache和Session的Driver

```
CACHE_DRIVER=redis
SESSION_DRIVER=redis
```

安装predis包

```
composer require predis/predis:^1.0
# code
\Cache::store('redis')->put('Laradock', 'Awesome', 10);
```

### 使用Mongo

```
docker-compose up -d mongo
# 配置Laravel的.env文件
INSTALL_MONGO=true
workspace:
    build:
        context: ./workspace
        args:
            - INSTALL_MONGO=true
...
php-fpm:
    build:
        context: ./php-fpm
        args:
            - INSTALL_MONGO=true
...
# 重建
docker-compose build workspace php-fpm
# 也可以在config/database.php配置
'connections' => [

    'mongodb' => [
        'driver'   => 'mongodb',
        'host'     => env('DB_HOST', 'localhost'),
        'port'     => env('DB_PORT', 27017),
        'database' => env('DB_DATABASE', 'database'),
        'username' => '',
        'password' => '',
        'options'  => [
            'database' => '',
        ]
    ],

    // ...

],
# 配置laravel
DB_HOST=mongo
DB_PORT=27017
# 安装类包
composer require jenssegers/mongodb
```

### 使用PhpMyAdmin

```
# use with mysql
docker-compose up -d mysql phpmyadmin
PMA_DB_ENGINE=mysql

# use with mariadb
docker-compose up -d mariadb phpmyadmin
PMA_DB_ENGINE=mariadb

http://localhost:8080
```

### 使用Adminer

```
docker-compose up -d adminer  
http://localhost:8080
```

> Adminer的版本固定在4.3.0 , 可以配置`adminer:latest`安装最新版

### 使用PgAdmin

```
docker-compose up -d postgres pgadmin
http://localhost:5050
```




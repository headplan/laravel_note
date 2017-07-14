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

### 工作区扩展

> 下面的工具都安装在workspace中 , 所以都需要对工作区进行重建

#### 全局安装Composer

```
# 配置
COMPOSER_GLOBAL_INSTALL=true
# 现在就可以安装全局包了,配置工作区下的composer.json
# 重建
docker-compose build workspace
```

#### 安装Prestissimo

这是一个提速composer的包 , 让Composer支持并行下载

有了全局的composer , 可以直接进入工作区安装 , 或者配置workspace/composer.json , 然后重建

```
docker-compose build workspace
```

#### 安装Node+NVM

> NVM是NodeJs版本管理工具,管理Nodejs版本和NPM版本

开启配置INSTALL\_NODE=true

重建工作区即可

#### 安装Node + YARN

方法同上 , 但要同时开启INSTALL\_NODE和INSTALL\_YARN

```
# 重建
docker-compose build workspace
```

#### 安装Linuxbrew

> 这是Linux版本的Homebrew
>
> http://linuxbrew.sh/

配置INSTALL\_LINUXBREW , 之后重建

#### 工作区Aliases别名

启动Docker的时候 , 会自动复制workspace文件夹下的aliases.sh文件 , 引入到.bashrc中 , 所以可以自定义你的快捷键了 . 

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

### 使用Beanstalkd

> 分布式内存队列系统

```
docker-compose up -d beanstalkd
# 配置config/queue.php
QUEUE_HOST=beanstalkd
# 用Composer引入包
https://github.com/pda/pheanstalk
http://localhost:2080/
```

### 使用ElasticSearch

```
docker-compose up -d elasticsearch
# http://localhost:9200
# 安装ES的插件
docker exec {container-name} /usr/share/elasticsearch/bin/plugin install delete-by-query
# 重启容器
docker restart {container-name}
```

### 使用Selenium

自动化测试工具

```
docker-compose up -d selenium
http://localhost:4444/wd/hub
```

### 使用RethinkDB

一个实时数据库 , 这有个Laravel包

tps://github.com/duxet/laravel-rethinkdb

```
docker-compose up -d rethinkdb
# 配置一下就可以了
```

### 使用Minio

分布式对象存储服务器

在工作区启动INSTALL\_MC

配置MINIO\_ACCESS\_KEY和MINIO\_ACCESS\_SECRET

这个现在在yml文件中了

```
docker-compose up -d minio
http://localhost:9000
mc mb minio/bucket
  S3_HOST=http://minio
  S3_KEY=access
  S3_SECRET=secretkey
  S3_REGION=us-east-1
  S3_BUCKET=bucket
```

> ### 安装CodeIgniter 3
>
> ```
> # 修改配置
> CODEIGNITER=true
> # 重建容器
> docker-compose build php-fpm
> ```

---

### 安装Aerospike扩展

> Aerospike\(以下简称AS）是一个以分布式为核心基础，可基于行随机存取内存中索引、数据或SSD存储中数据的数据库。它主要用于百G、数T等大数据量并且在数万以上高并发情况下，对性能也有ms读取插入要求的场景。目前主要集中于互联网广告行业，如eXelate、BlueKai、MediaV、 InMobi、 applovin等。

配置工作区和PHP-FPM中的INSTALL\_AEROSPIKE为true

重建即可

```
docker-compose build workspace php-fpm
```




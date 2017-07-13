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




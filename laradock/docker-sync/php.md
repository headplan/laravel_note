# PHP相关

#### 安装PHP扩展

由于PHP的FPM和CLI在不同的容器中 , 所以需要编辑两个地方 , 当然只编辑一个容器也可以 . 

* PHP-FPM扩展应该安装在php-fpm/Dockerfile-XX
* PHP-CLI扩展应该安装在workspace/Dockerfile

#### 更换PHP-FPM版本

.env文件中默认配置运行php7.1版本 . 

PHP相关的Dockerfile文件准备了3个版本 , 7.1,7.0,5.6 . 

如果只切换PHP-FPM版本 , 修改docker-compose.yml文件

```
php-fpm:
        build:
            context: ./php-fpm
            dockerfile: Dockerfile-70
    ...
```

然后重建容器即可

```
docker-compose build php-fpm
```

要切换到其他没有Dockerfile的版本的PHP , 可以自己改改Dockerfile , 或者去官网提供的Dockerfile看看 . 

> https://hub.docker.com/\_/php/

#### 更换PHP-CLI版本




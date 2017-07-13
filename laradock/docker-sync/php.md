# PHP相关

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

> [https://hub.docker.com/\_/php/](https://hub.docker.com/_/php/)

#### 更换PHP-CLI版本

这里和上面的方法一样 , PHP-CLI的Dockerfile在工作区中 , 也就是workspace目录中 .

#### 安装PHP扩展

由于PHP的FPM和CLI在不同的容器中 , 所以需要编辑两个地方 , 当然只编辑一个容器也可以 .

* PHP-FPM扩展应该安装在php-fpm/Dockerfile-XX
* PHP-CLI扩展应该安装在workspace/Dockerfile

**安装xDebug**

其实.env文件中已经设置了很多变量开关给docker-compose.yml文件用 , 可以自己添加或者直接修改yml文件的开关为true .

```
workspace:
    build:
        context: ./workspace
        args:
            - INSTALL_XDEBUG=true
...
php-fpm:
    build:
        context: ./php-fpm
        args:
            - INSTALL_XDEBUG=true
...
```

记得修改后重建一下

```
docker-compose build workspace php-fpm
```

还有配置文件可以修改

* `laradock/workspace/xdebug.ini`
* `laradock/php-fpm/xdebug.ini`

xdebug还准备了开关脚本 , 可以直接在文件夹中启动关闭查看状态

```
.php-fpm/xdebug {stop|start|status}
```

> 可能需要chmod给执行访问权限

有关phpstorm中使用xdebug的内容查看phpstorm相关笔记 .

**PHP部署工具Deployer**

Workspace中也有开关 , 开启即可 . 

Deployer文档 : https://deployer.org/docs


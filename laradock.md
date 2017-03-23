# LaraDock

影响速度的某些原因,需要先给Docker换上国内源,阿里云中可以申请地址.

Preferences &gt; Daemon &gt; Basic &gt; Registry mirrors 中添加.

在workspace容器中,修改Dockerfile文件,搜索`apt-get update`在其上面先设置ubuntu源

```
sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
```

除了这些,还有其他影响速度的,类似composer源,NPM等等,可根据情况修改.

---

```
# 克隆laradock
git clone https://github.com/Laradock/laradock.git
# 进入文件夹,运行docker
docker-compose up -d nginx mysql redis beanstalkd
```

如果已经有一个Laravel项目,可以克隆到项目根目录

```
git submodule add https://github.com/laradock/laradock.git
```

可以选择自己的容器组合

nginx, hhvm, php-fpm, mysql, redis, postgres, mariadb, neo4j,

mongo, apache2, caddy, memcached, beanstalkd, beanstalkd-console, workspace等.

> 注:`workspace`和`php-fpm`将运行在大部分实例中, 所以不需要在`up`命令中加上它们.

启动之后就可以进入`workspace`容器了,执行Laravel安装及Artisan命令等操作.

```
# 以laradock用户的身份进入容器
docker-compose exec —user=laradock workspace bash
```

编辑`laradock`目录下的`docker-compose.yml`文件修改目录映射.

---

**Docker常用命令**

1.列出正在运行的容器

```
docker ps
```

你也可以使用以下命令列出某项目的容器：

```
docker-compose ps
```

2.启动容器

```
docker-compose up -d {容器名称}
```

3.关闭所有容器

```
docker-compose stop
```

关闭某个容器：

```
docker-compose stop {容器名称}
```

4.删除所用容器

```
docker-compose down
```

> 使用该命令要小心, 因为它会删除数据容器.

5.进入容器

首先使用`docker ps`查看正在运行的容器，然后进入其中某个容器：

```
docker-compose exec {container-name} bash
```

例如, 进入MySQL容器

```
docker-compose exec mysql bash
```

要退出容器, 执行`exit`即可.

6.编辑容器默认配置

编辑docker-compose.yml文件.

例如,修改MySQL数据库名称

```
environment:
    MYSQL_DATABASE: laradock
```

修改Redis端口号

```
ports:
    - "1111:6379"
```

7.编辑Docker镜像

修改dockerfile文件,例如MySQL位于mysql/Dockerfile

重新构建容器`docker-compose build mysql`, 或运行`docker-compose build`构建所有容器

```
docker-compose build {container-name}
# 重构容器,不使用缓存
docker-compose build --no-cache {container-name}
```

8.查看日志文件

查看容器的日志, 可以运行命令:

```
docker logs {container-name}
```

---

Workspace容器可以执行像Artisan, Composer, PHPUnit, Gulp等命令.

```
# 进入容器
docker-compose exec workspace bash
# 使用特定账户进入容器
docker-compose exec --user=laradock workspace bash
```

> 可以从docker-compose.yml文件修改PUID\(User id\)和PGID\(group id\)值.

**\[Laravel\]**

从Docker镜像安装Laravel

**进入Workspace容器安装Laravel镜像**

> 1.进入Workspace容器
>
> ```
> docker-compose exec workspace bash
> ```
>
> 2.安装Laravel
>
> ```
> composer create-project laravel/laravel my-cool-app "5.2.*"
> ```
>
> 3.编辑docker-compose.yml映射新的应用目录
>
> ```
> application:
>         build: ./application
>         volumes:
>             - ../my-cool-app/:/var/www
> ```

现在就可以进入映射目录, 直接编辑文件了.

**进入Workspace容器运行命令**

查看运行的容器

```
docker-compose ps
```

进入Workspace容器

```
docker-compose exec --user=laradock workspace bash
```

现在就可以执行常用的命令了

```
php artisan
composer
phpunit
```




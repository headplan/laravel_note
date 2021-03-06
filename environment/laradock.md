# LaraDock\(更新中...\)

**官网** - [http://laradock.io/](http://laradock.io/)

**Github** - [https://github.com/laradock/laradock](https://github.com/laradock/laradock)

---

### 介绍

LaraDock - 一个Docker驱动的完整的PHP开发环境 .

**快速启动**

```
# 克隆仓库
git clone https://github.com/Laradock/laradock.git
# 创建.env
cp env-example .env
# 启动容器
docker-compose up -d nginx mysql redis beanstalkd
# 编辑.env文件
DB_HOST=mysql
REDIS_HOST=redis
QUEUE_HOST=beanstalkd
# 访问
http://localhost
# bingo
```

其他介绍...

### 入门

**依赖**

* Git
* Docker &gt;= 1.12

**安装**

如果已经有了项目寻找 , 可以添加submodule

```
git submodule add https://github.com/Laradock/laradock.git
# tree
project-a
-- laradock-a
project-b
-- laradock-b
```

> 注意 : 如果要在多个项目中运行laradock , 这里的laradock文件夹名字需要重命名为唯一的.

如果还没创建项目 , 可以直接clone

```
git clone https://github.com/laradock/laradock.git
# tree
laradock
project-dev
```

clone方式创建多个项目 , 需要配置nginx

```
laradock
project-1
project-2
# 添加nginx配置到NGINX_SITES_PATH文件夹中
# 方法和添加hosts一样,只要注意路径都是在APPLICATION下即可
```

### **使用**

**1.初始化laradock**

使用laradock的第一步肯定是要新建.env文件,配置laradock

```
cp env-example .env
```

可以直接编辑.env文件 , 配置应用 , 这些env中的变量都会在docker-compose.yml文件中使用 .

**2.生成环境并运行**

使用docker-compose启动

```
docker-compose up -d nginx mysql
```

大多数情况下 , workspace和php-fpm会自动运行 , 如果找不到可以运行

```
docker-compose up -d nginx php-fpm mysql workspace
```

**3.进入工作区**

启动之后就可以进入`workspace`工作区容器了 , 执行Laravel安装及Artisan , Composer , PHPUnit等命令操作 .

```
docker-compose exec workspace bash
# 还可以指定用户进入,可以在.env文件里修改用户ID(PUID)和组ID(PGID)变量
docker-compose exec --user=laradock workspace bash
```

**4.配置项目**

为PHP项目配合数据库主机 , 直接修改PHP项目的.env文件即可

```
DB_HOST=mysql
```

这里不用写IP地址 , 直接写mysql即可 .

**5.浏览器访问绑定host的域名**

---

影响速度的某些原因,需要先给Docker换上国内源,阿里云中可以申请地址.

Preferences &gt; Daemon &gt; Basic &gt; Registry mirrors 中添加.

在workspace容器中,修改Dockerfile文件,搜索`apt-get update`在其上面先设置ubuntu源

```
sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
```

除了这些,还有其他影响速度的,类似composer源,NPM等等,可根据情况修改.

---

### **Docker常用命令**

**1.列出正在运行的容器**

```
docker ps
docker ps -a # 列出所有容器

docker kill $(docker ps -a -q) # 杀死所有正在运行的容器
docker rm $(docker ps -a -q) # 删除所有已经停止的容器

=====

docker image -a # 列出所有镜像
docker rmi $(docker images -q) # 删除所有镜像
```

**2. 列出某项目的容器：**

```
docker-compose ps
# 启动容器
docker-compose up -d {容器名称}
# 关闭所有容器
docker-compose stop
# 关闭某个容器
docker-compose stop {容器名称}
# 删除所有容器,使用该命令要小心,因为它会删除数据容器.
docker-compose down
```

**3.进入容器**

首先使用`docker ps`查看正在运行的容器，然后进入其中某个容器：

```
docker-compose exec {container-name} bash
```

例如, 进入MySQL容器

```
docker-compose exec mysql bash
# 直接进入mysql
docker-compose exec mysql mysql -uroot -proot
```

要退出容器, 执行`exit`即可.

**4.编辑容器默认配置**

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

**5.编辑Docker镜像**

修改dockerfile文件,例如MySQL位于mysql/Dockerfile

重新构建容器`docker-compose build mysql`, 或运行`docker-compose build`构建所有容器

```
docker-compose build {container-name}
# 重构容器,不使用缓存
docker-compose build --no-cache {container-name}
```

**6.查看日志文件**

查看容器的日志, 可以运行命令:

```
docker logs {container-name}
```

---

### Docker-Sync\(文件同步\)

查看Laradock下相关内容

### 添加更多软件\(Docker Images\)

添加更多软件镜像 , 编辑docker-compose.yml文件 , 根据docker compose的配置语法写入即可 , 参考下面的文章或进入Docker Note笔记查看 .

[https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)

### 查看日志文件

Nginx日志文件存储在logs/nginx目录中 . 要查看所有容器的日志 , 可以使用命令

```
docker-compose logs {container-name}
docker-compose logs -f {container-name}
docker-compose logs --help # 查看更多选项
```

### **\[Laravel\]**

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

**使用自定义域名**

编辑/etc/hosts文件

```
127.0.0.1 lartisan.dev
```

配置Nginx

```
# 配置文件在目录下
laradock/nginx/sites
# 也可以进入nginx容器中修改
docker-compose exec nginx bash
# 添加
server_name lartisan;
```

**开启全局Composer命令**

为启用全局Composer Install在容器构建中允许安装composer的依赖, 编辑docker-compose.yml文件

```
# 修改配置文件为true
COMPOSER_GLOBAL_INSTALL=${WORKSPACE_COMPOSER_GLOBAL_INSTALL}
# 这个有问题
```

然后添加依赖关系到workspace/composer.json文件中

比如添加**Prestissimo**到平行安装功能的composer插件

```
hirak/prestissimo": "^0.3"
```

重建Workspace容器即可

```
docker-compose build workspace
```

**安装Node + NVM**

编辑docker-compose.yml文件.

在Workspace容器找到INSTALL\_NODE选项设为true

重建容器

```
docker-compose build workspace
```

### 常见问题与升级

**问题**

看到空白页,而不是Laravel的欢迎页面.

在Laravel根目录给权限即可

```
sudo chmod -R 777 storage bootstrap/cache
```

看到 “Welcome to nginx” 而不是 Laravel 应用

> 在浏览器使用[http://127.0.0.1](http://127.0.0.1替换http://localhost)替换[http://localhost](http://127.0.0.1替换http://localhost)

看到包含address already in use的错误

> 确保你想运行的服务端口\(80, 3306, etc.\)不是已经被其他程序使用, 例如apache/httpd服务或其他安装的开发工具

**升级**

停止Docker虚拟机

```
docker-machine stop {default}
```

升级LaraDock

```
git pull origin master
```

像之前一样启动

```
docker-compose up -d nginx mysql
```

> 如果遇到问题, 重置容器, 但是容器数据可能会丢失
>
> docker-compose build --no-cache

### MySQL, Redis等配置

**MySQL**

配置端口号


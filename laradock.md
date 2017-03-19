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




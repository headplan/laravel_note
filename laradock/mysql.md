# MySQL

#### 从主机访问MySQL

可以将 mysql/MariaDB 端口转发到您的主机 , 配置yml文件即可 , 或者加载自己定义的yml文件

```
ports:
    - "3306:3306"
```

#### 访问MySQL

进入mysql

```
docker-compose exec mysql bash
mysql -uroot -proot
mysql -uhomestead -psecret
SELECT User FROM mysql.user;
show databases; ...
```

#### 创建多个数据库

初始化的数据库创建文件在

```
mysql/docker-entrypoint-initdb.d/*
```

可以编辑这个文件 , 创建更多的数据库 . 

#### 更改MySQL端口

编辑mysql/my.cnf文件中的port

```
[mysqld]
port=1234
```

别忘记修改配置文件映射的端口也要修改 . 




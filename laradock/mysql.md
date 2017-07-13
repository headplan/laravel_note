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




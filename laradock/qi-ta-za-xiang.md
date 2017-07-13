# 其他杂项

#### 更改时区

可以直接配置.env的WORKSPACE\_TIMEZONE参数 , 也可以配置yml文件

```
workspace:
    build:
        context: ./workspace
        args:
            - TZ=America/New_York
...
```

#### 添加定时任务

进入workspace容器添加

```
* * * * * php /var/www/artisan schedule:run >> /dev/null 2>&1
* * * * * root echo "Every Minute" > /var/log/cron.log 2>&1
```

#### 通过SSH访问工作区容器

设置INSTALL\_WORKSPACE\_SSH就可以通过localhost:2222访问工作区容器 : 

```
workspace:
ports:
    - "2222:22" # Edit this line
...
```




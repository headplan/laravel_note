# 高级任务配置

前面的配合中 , 高级配置有四个任务选项可以填写 :

**pre\_deploy任务**

```
echo pre_deploy >> /tmp/cmd        # 初始化一些东西，自由发挥
```

**post\_deploy任务**

```
mvn package -Dmaven.test.skip=true # 编译java
mvn clean                          # 打扫
mv WEB-INF/config.Properties.test WEB-INF/config.Properties # 切换环境相应的配置
rm -rf src                         # 甚至删除无用代码
```

**pre\_release任务**

```
./xx.sh stop                       # 暂停服务
```

**post\_release任务**

```
./xx.sh start                      # 启动服务
```

如果想执行`sudo`命令 , 前提是用户有root权限 . 

添加用户到sudoers

```
visudo
www    ALL=(ALL)       ALL
```

添加免密码命令

```
visudo
www ALL = (ALL) NOPASSWD: /usr/local/nginx/bin/nginx
```

设置用户的tty（宿主机执行sudo需要此步，目标机可以跳过此步）

```
Defaults:www    !requiretty
```




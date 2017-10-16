# Git项目配置

#### **基本要求**

宿主机php进程用户walle的ssh-key , 要加入git/gitlab的deploy-keys中 .

宿主机php进程用户walle的ssh-key , 要加入目标机群部署用户www的ssh-key信任 .

> 可通过配置的检测查看或ps aux \| grep php

**可遇见的错误**

宿主机代码检出检测出错 , 请确认php进程用户{user}有代码存储仓库{path}读写权限 , 且把ssh-key加入git的deploy-keys列表 .

* 问题：**请确认php进程用户{user}有代码存储仓库{path}读写权限**

```
没有权限，是因为用户{user}对目录{path}没有读写权限，给权限即可
ll {path}
chown {user} -R {path}
chmod 755 -R {path}
```

* 问题：**把ssh-key加入git的deploy-keys列表**

```
su {user} && cat ~/.ssh/id_rsa.pub
打开 github/gitlab/bitbucket 网站, 添加 ssh-key 到ssh-keys列表
```

如果还会报错 , 可能需要手工git clone一次 :![](/assets/import111111.png)

目标机器部署出错 , 请确认php进程{local\_user}用户ssh-key加入目标机器的{remote\_user}用户ssh-key信任列表 , 且{remote\_user}有目标机器发布版本库{path}写入权限 .

解决方案同上 , 其中可能遇到目标机器切换时`This account is currently not available`的情况 :

```
cat /etc/passwd | grep www
www:x:501:501::/home/www:/sbin/nologin
修改为
www:x:501:501::/home/www:/bin/bash
或者使用参数(经过测试,必须像上面那样修改才有效)
su --shell=/bin/bash www
```

如果没有家目录 , 新建并给权限即可 .

#### 配置项目

![](/assets/walle.png)配置完成后检测通过 , 如无问题则可以发起上线单了 . 


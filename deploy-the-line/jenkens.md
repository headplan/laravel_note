# Jenkins

官网 : [https://jenkins.io/](https://jenkins.io/)

下载 : [https://jenkins.io/download/](https://jenkins.io/download/)

相关资料 :

[https://jenkins.io/doc/](https://jenkins.io/doc/)

[http://www.jenkins.org.cn/](http://www.jenkins.org.cn/)

[https://www.w3cschool.cn/jenkins/](https://www.w3cschool.cn/jenkins/)

---

**MacOS安装**

直接下载安装按照步骤即可 .

**Redhat系列安装**

使用仓库

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

然后直接yum安装

```
yum install jenkins
```

安装完成后的文件位置

```
/var/lib/jenkins/ -- 默认的JENKINS_HOME目录
/usr/lib/jenkins/jenkins.war -- WAR包
/etc/sysconfig/jenkins -- 配置文件
/var/log/jenkins/jenkins.log -- Jenkins日志文件
```

**启动服务**

```
sudo service jenkins start
提示错误[FAILED],查看细节
systemctl status jenkins.service
```

一般是因为没有配置java , 测试一下

```
java -version
```

**安装java**

```
yum install java
```

安装了java之后 , 就可以直接启动了 , 如果无法启动 , 可以修改配置文件 :

```
# 配置文件
/etc/init.d/jenkins
# 设置配置文件,查找
candidates="
```

**启动服务**

Linux下jenkins默认使用jenkins用户进行脚本和文件的操作 , 如果不修改 , 在部署项目时需要调整涉及到的文件和目录的操作权限 , 可以调整jenkins配置文件 , 将用户修改为root用户 .

```
vi /etc/sysconfig/jenkins 配置文件
将JENKINS_USER="jenkins"调整为JENKINS_USER="root"
也可以修改端口,默认为8080.
```

**通过IP访问**

在本地浏览器中输入ip:8080访问 . 第一次登录Jenkins会要求解锁 . 查看 :

```
cat /var/lib/jenkins/secrets/initialAdminPassword
```

#### 解决：安装Jenkins时web界面出现该jenkins实例似乎已离线

解锁后安装插件 , 如果访问状态为离线 , 是因为访问默认是https请求 , 可以直接访问下面的地址修改 :

```
pluginManager/advanced
```

修改为http请求 :

```
https://updates.jenkins.io/update-center.json
to
http://updates.jenkins.io/update-center.json
```

也可以修改/var/lib/jenkins/hudson.model.UpdateCenter.xml文件中的这个地址 , 原因都是https的问题 .

下面是两个国内备用地址 :

```
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
http://mirror.esuni.jp/jenkins/updates/update-center.json
```

jenkins在下载插件之前会先检查网络连接 , 其会读取这个文件中的网址 . 默认是访问谷歌 , 所以还是修改成百度吧 .

```
/var/lib/jenkins/updates/default.json
```

重启服务 :

```
sudo service jenkins restart
```

选择推荐的插件 , 最后安装完成 .


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

启动服务

```
sudo service jenkins start
提示错误[FAILED],查看细节
systemctl status jenkins.service
```

一般是因为没有配置JAVA环境


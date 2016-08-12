# Centstead

虽然这个也不错,但还是用了自己搭建的VagrantBox.

这是一个Fork了Homestead的项目,功能几乎和Homestead一样.

**Centstead 预置软件： \(可基于配置快速切换版本\)**

* CentOS 7
* PHP 5.4 \/ 5.5 \/ 5.6 \/ 7.0
* Mysql 5.5 \/ 5.6 \/ 5.7
* MariaDB 5.5 \/ 10 \/ 10.1
* PostgreSQL 9.2 \/ 9.3 \/ 9.4 \/ 9.5
* Composer
* Nginx
* Redis
* Sqllite3
* Supervisor
* Xdebug
* Nodejs \(npm webpack gulp bower\)
* memcached
* Beanstalkd

## **安装与设置**

Centstead 盒子提供了两个版本:

1. 先锋版 virtualbox\_avant.box ,预制 php 7.0 mysql 5.7
2. 通用版 virtualbox\_usual.box ,预制 php 5.6 mysql 5.6

盒子下载百度网盘:[https:\/\/pan.baidu.com\/s\/1c15ybAS](https://pan.baidu.com/s/1c15ybAS)

**添加盒子**

cd downlods\/path

vagrant box add .\/avant.box --name=jason-chang\/centstead-avant

或者

vagrant box add .\/usual.box --name=jason-chang\/centstead-usual

**通过 GitHub 克隆 Centstead**

git clone https:\/\/github.com\/jason-chang\/centstead.git centstead

**初始化配置文件**

在centstead目录下运行init.sh\/init.bat执行初始化操作.

**bash init.sh**

初始化过程会创建config文件夹,并生成两个文件:

**config.yaml** - 主配置文件\(本地添加盒子的话,初始化后去掉配置文件version前的注释\)

**after.sh** - vagrant provision 完成后的钩子文件,可以用来执行自定义虚拟环境操作

**注意:**

执行init脚本过程中需要安装部分vagrant插件,安装可能会应为GFW被墙,可以打开蓝灯试试.

但是全局的代理对bash\/cmd是不生效的,所以使用命令

set http\_proxy=[http:\/\/127.0.0.1:1080\/](http://127.0.0.1:1080/)

端口以蓝灯为准

## **配置Vagrant Box**

**基础配置**

* ip - 虚拟机ip
* memory:虚拟机内存
* cpus:虚拟机cpu核心数
* provider:虚拟机容器,现在使用的是virtualbox
* box:基础盒子名称
* version:基础盒子版本,此项默认关闭\/注释,离线安装盒子的用户请开启

```
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox
box: jason-chang/centstead-usual
#version: ">=0"
```

**配置共享文件夹**

* map:主机文件目录

* to:映射到虚拟机的目录

* type:同步类型

  * 默认为空表示使用shareFolder
  * nfs\(推荐\)表示使用nfs同步\(nfs:网络文件系统\) 


```
folders:
 - map: ~/Projects
 to: /home/vagrant/Projects
 type: nfs
```

**Nginx网站配置**

* conf:项目最终保存成的配置文件

* servers:项目包含的域名servers

* map:支持的域名server name 多个域名可以空格分隔

* to:指向的项目根入口目录

* type:站点类型,支持的配置值

  * 不设置\/default\(普通php站index.php为入口\)
  * static\(静态文件站\)
  * proxy\(反向代理站\)
  * laravel\(项目网站\)
  * thinkphp\(thinkphp网站\)
  * symfony\(symfony2网站\)

* port:http监听端口

* ssl:https监听端口
* aliases:所有在 centstead 中配置的站点 centstead 将智能的加入主机的 hosts 文件中,这样当 vagrant 启动完成的时候后,可以直接浏览配置的域名了.

  * centstead 默认取 map 值加入 hosts 文件，但是由于 hosts 文件并不是非常强大, 类似 `.demo.app的泛解析域名将不能支持,aliases就是为此提供的自定义接口, 如果配置存在aliases则抓取aliases加入hosts` 文件


```
sites:
    - conf: demo.app
      servers:
       - map: static.demo.app
         to: /home/vagrant/Projects/Demo/static
         type: static

       - map: "demo.app *.demo.app"
         to: /home/vagrant/Projects/Demo 
         aliases: "demo.app a.demo.app u.demo.app www.demo.app" 
         # port: 80 
         # ssl: 443
```

默认情况每个站点都可以通过HTTP:8000和HTTPS:44300访问

现在就可以vagrant up启动盒子了,已经启动过的环境执行vagrant provision应用新的站点配置,相当于修改了配置之后执行使配置生效.


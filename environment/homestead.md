# **Homestead**

> 作为官方推荐的本地开发环境 , 这里再次统一整理一次 . 前期和LaraDock一样 , 坑很多所以放弃 , 经历过这么多更新后 , 希望一切顺利 .
>
> 使用Homestead的好处不再冗述 , 直接上流程\(MacOS\) .
>
> Laravel中国上已有很好的总结 , 可以参考 : 
>
> https://laravel-china.org/docs/laravel-development-environment/5.5

#### 安装 VirtualBox

**下载地址**

[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

当前版本 : 5.1.28

#### 安装 Vagrant

**下载地址**

[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

当前版本 : 2.0.0

#### Homestead 介绍

Homestead 利用 Vagrantfile 提供的便利，定制了一整套的可配置、可移植和复用的 Laravel 开发环境 .

Homestead 包含了两个东西：

* Homestead 管理脚本
* Homestead Box 虚拟机盒子

**Homestead 管理脚本**

其使用 Ruby 和 Shell 脚本编写而成。原理是对 Vagrantfile 文件做定制。将从 `~/Homestead/Homestead.yaml` 读取的配置信息，在 provision 时，解析为 Vagrant 命令并进行对虚拟机的配置。Homestead 脚本的作用在于，提供了极其简单易用的接口，使我们只需要通过傻瓜化配置，即可完成复杂的任务。以下是几个常用的任务：

* IP 配置，端口映射；
* Nginx Site 创建；
* 数据库创建；
* 主机文件夹挂载到虚拟机等任务。

历史版本在这里 :

[https://github.com/laravel/homestead/releases](https://github.com/laravel/homestead/releases)

**Homestead Box 虚拟机盒子**

`homestead.box`虚拟机盒子是提前打包好的 Vagrant Box 虚拟机盒子 , 里面预装了 Nginx Web 服务器、PHP等Laravel开发时所需要用到的各种软件 .

虚拟机盒子的历史版本 :

[https://atlas.hashicorp.com/laravel/boxes/homestead/](https://atlas.hashicorp.com/laravel/boxes/homestead/)

#### Homestead 安装和使用

**下载和导入 Homestead Box**

这里准备了适用于国内的Box , 包括Composer,NPM和Yarn的国内加速镜像 .

```
vagrant box add metadata.json
==> box: Successfully added box 'laravel/homestead' (v3.0.0) for 'virtualbox'!
vagrant box list
laravel/homestead (virtualbox, 3.0.0)
```

> 更多关于vagrant的命令 , 可以到Linux\_note中的专题查看 .
>
> 这里metadata.json只是一个配置文件 , 可以自定义 .

box导入完成 .

#### 下载 Homestead 管理脚本

官方下载地址 : [https://github.com/laravel/homestead](https://github.com/laravel/homestead)

检出放到`~/Homestead`目录下 .

```
cd ~
git clone https://github.com/laravel/homestead.git Homestead
cd ~/Homestead
git checkout v5.4.0
```

这里 , 我们使用v5.4.0版本 . 然后 , 初始化 Homestead :

```
bash init.sh
```

初始化后 , 会在 `~/Homestead`目录下生成以下三个文件 :

* Homestead.yaml - 主要配置信息文件 , 我们可以在此文件中配置 Homestead 的站点和数据库等信息 ; 
* after.sh - 每一次 Homestead 盒子重置后\(provision\)会调用的 shell 脚本文件 ; 
* aliases - 每一次 Homestead 盒子重置后\(provision\), 会被替换至虚拟机的`~/.bash_aliases`
  文件中 , `aliases`里可以放一些快捷命令的定义 . 

#### Homestead.yaml 配置文件

**虚拟机设置**

```
# 保持默认即可
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox
```

**SSH 秘钥登录配置**

```
authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa
```

* `authorize`选项是指派登录虚拟机授权连接的公钥文件 , 此文件填写的是主机上的公钥文件地址 , 虚拟机初始化时 , 此文件里的内容会被复制存储到虚拟机的 `/home/vagrant/.ssh/authorized_keys`文件中 , 从而实现 SSH 免密码登录 . 
* `keys`是数组选项 , 填写的是本机的 SSH 私钥文件地址 . 虚拟机初始化时 , 会将此处填写的所有 SSH 私钥文件复制到虚拟机的`/home/vagrant/.ssh/`文件夹中 , 从而使虚拟机能共享主机上的 SSH 私钥文件 , 使虚拟机具备等同于主机的身份认证 . 此功能为 SSH 授权提供了便利 . 例如 , 我们在Github上配置了公钥 , 这种同步即可实现 GitHub 对虚拟机和主机共同认证 . 

这里我们创建Homestead专用的密钥对 :

```
ssh-keygen -t rsa -C "headplan@163.com"
```

当然 , 这里需要配置一下ssh的config

```
Host 192.168.10.10
    IdentityFile ~/.ssh/homestead
```

修改一下配置文件\(这里把公钥也同步上去\) :

```
authorize: ~/.ssh/homestead.pub

keys:
    - ~/.ssh/homestead
    - ~/.ssh/homestead.pub
```

**共享文件夹配置**

`folders`指明了本机要映射到 Homestead 虚拟机上的文件夹 .

* `map`对应的是我们本机的文件夹
* `to`对应的是 Homestead 上的文件夹

**站点配置**

也是映射的概念

```
sites:
    - map: homestead.app
      to: /home/vagrant/Code/Laravel/public
```

要访问虚拟机站点还要绑定hosts :

```
192.168.10.10  homestead.app
```

**数据库配置**

这里只是指定数据库名称 :

```
databases:
    - homestead
```

**自定义变量**

如果需要自定义一些在虚拟机上可以使用的自定义变量 , 则可以在`variables`中进行定义 :

```
variables:
    - key: APP_ENV
      value: local
```

#### 运行 Vagrant

**常见命令**

* vagrant init - 初始化 vagrant
* vagrant up - 启动 vagrant
* vagrant halt - 关闭 vagrant
* vagrant ssh - 通过 SSH 登录 vagrant（需要先启动 vagrant）
* vagrant provision - 重置应用更改 vagrant 配置
* vagrant destroy  - 删除 vagrant

或者使用Vagrant Manager .

启动 Homestead :

```
cd ~/Homestead && vagrant up
```

第一次启动时 , Vagrant 会做以下这几件事情 :

* 以导入的 Homestead 虚拟机盒子为模板 , 新建一台虚拟机
* 并按照 `Homestead.yaml`里的配置信息 , 对这台新建的虚拟机进行配置
* 配置完成后启动虚拟机

和操作Vagrant一样 , 进入Homestead目录 , SSH登录 :

```
vagrant ssh
exit - 退出
vagrant halt - 关闭虚拟机
```

#### Hello Laravel !

进入虚拟机

```
> cd ~/Homestead && vagrant up
> vagrant ssh
```

新建一个名为 Laravel 的项目

```
cd ~/Code
composer create-project laravel/laravel Laravel --prefer-dist "5.5.*"
```

访问[http://homestead.app/](http://homestead.app/)

安装完成


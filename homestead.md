# **Homestead**

> 作为官方推荐的本地开发环境 , 这里再次统一整理一次 . 前期和LaraDock一样 , 坑很多所以放弃 , 经历过这么多更新后 , 希望一切顺利 .
>
> 使用Homestead的好处不再冗述 , 直接上流程\(MacOS\) .

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

版本历史在这里 : 

https://github.com/laravel/homestead/releases




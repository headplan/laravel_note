# 瓦力walle

#### 依赖

* Bash\(git、ssh\)
  * 意味着不支持win、mac的zsh
* LNMP/LAMP\(php5.4+\)
  * php需要开启pdo\_mysql，exec函数执行
* Composer
  * 如果国内环境安装极慢，可以直接下载[vendor](http://pan.baidu.com/s/1c0wiuyc)解压到项目根目录
* ansible

1、宿主机安装 ansible

```
yum install ansible # RHEL/CentOS/Fedora
apt-get install ansible # Debian/Ubuntu
emerge -avt ansible  # Gentoo/Funtoo
pip install ansible # will also install paramiko PyYAML jinja2
# 可以使用阿里云源
pip3 install ansible -i https://mirrors.aliyun.com/pypi/simple/
```

2、宿主机无需其他配置，兼容 ~/.ssh/config 名称、证书配置

3、目标机无需额外配置

> 1. 项目配置 中 开启Ansible
> 2. \(可选\) config/params.php 配置 ansible\_hosts 文件存放路径
> 3. 按正常流程发布、上线代码，传输文件、远程执行命令均会通过ansible并发执行

#### 安装


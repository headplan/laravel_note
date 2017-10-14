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

```
git clone git@github.com:meolu/walle-web.git
```

设置MySQL数据库连接

```
vi config/local.php
'db' => [
    'dsn'       => 'mysql:host=127.0.0.1;dbname=walle', # 新建数据库walle
    'username'  => 'username',                          # 连接的用户名
    'password'  => 'password',                          # 连接的密码
],
```

composer 安装依赖

```
composer install --prefer-dist --no-dev --optimize-autoloader -vvvv
安装速度慢或失败,可直接下载官网提供的vendor解压到项目根目录
```

执行初始化命令

```
# 别忘记创建数据库
./yii walle/setup
```

然后nginx简单配置一下即可

```
server {
    listen       80;
    server_name  walle.compony.com; # 改你的host
    root /the/dir/of/walle-web/web; # 根目录为web
    index index.php;

    # 建议放内网
    # allow 192.168.0.0/24;
    # deny all;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri = 404;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```




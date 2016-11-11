# Blog开发流程记录

### 安装

**安装composer**

> **根据官方命令安装Comoser**
> 
> php -r "copy\('https:\/\/getcomposer.org\/installer', 'composer-setup.php'\);"
> 
> php -r "if \(hash\_file\('SHA384', 'composer-setup.php'\) === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae'\) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink\('composer-setup.php'\); } echo PHP\_EOL;"
> 
> php composer-setup.php
> 
> php -r "unlink\('composer-setup.php'\);"
> 
> 最新的安装方式可以在 [https:\/\/getcomposer.org\/download\/](https://getcomposer.org/download/) 查看
> 
> 安装之前需要配置为了兼容Composer的php.ini
> 
> vim \/etc\/php.d\/15-xdebug.ini \# 注释掉扩展
> 
> vim \/etc\/php.ini \# 去掉两个禁用的函数
> 
> proc\_open,proc\_get\_status
> 
> \# 设置全局通用
> 
> mv composer.phar \/usr\/bin\/composer

配置中国全量镜像

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com "全局"
```

```
composer config repo.packagist composer https://packagist.phpcomposer.com "单个项目"
在composer.json中加入
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```

**安装laravel**

项目安装查看文档前面的"系统安装"里的内容,这里要注意的是:

> composer create-project laravel\/laravel blog "5.2.\*" --prefer-dist
> 
> blog "5.2.\*" - 指定安装的文件夹和版本
> 
> --prefer-dist - 强制使用压缩包安装而不是克隆源码

或者直接下载zip压缩包或者github下载.

**初始化laravel**

配置nginx指定目录到项目目录,指定默认访问文件为server.php

或者指定目录到public,前提是:

> 独立服务器,有修改入口文件目录权限或者子目录绑定域名的情况下

配置rewrite\(apache直接使用public下的.htaccess\)

> location \/ { try\_files **$uri** **$uri**\/ \/index.php?**$query\_string**; }

检查php.ini文件下列扩展是否开启

* php\_openssl
* php\_mbstring
* php\_pdo\_mysql

> 如果使用压缩包解压安装,需要重新生成key,运行下面的命令:
> php artisan key:generate

参考文档准备:http:\/\/laravelacademy.org\/laravel-docs-5\_2



基础

开发

部署,备份,维护


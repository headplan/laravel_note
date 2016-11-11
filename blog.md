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

项目安装查看文档前面的"系统安装"里的内容,这里要注意的是:

> composer create-project laravel\/laravel blog "5.2.\*" --prefer-dist
> 
> blog "5.2.\*" - 指定安装的文件夹和版本
> 
> --prefer-dist - 指定强制使用压缩包安装而不是克隆源码

基础

开发

部署,备份,维护


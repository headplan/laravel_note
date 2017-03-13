# Composer

在PHPNOTE中已经有基本完整的记录,下面记录的是希望形成习惯的流程.

简单理解,Composer就是JS中的npm,Ruby中的Bundler,包依赖管理.

#### 安装

```php
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

#### 安装配置

```
# 安装路径
php composer-setup.php --install-dir=bin
# 安装文件名
php composer-setup.php --filename=composer
# 安装版本
php composer-setup.php --version=1.0.0-alpha8
```

> **TODO**  
> publickey - [https://composer.github.io/pubkeys.html](https://composer.github.io/pubkeys.html)
>
> 脚本安装 - [https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md](https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md)
>
> 最新测试版本使用参数 - composer self-update --preview --snapshot

#### Package引用和版本

编辑composer.json文件

```
{
  "require":{
    "mustache/mustache":"2.9.0"
  }
}
composer install
```

命令行引入

```
composer require mustache/mustache
```

版本更新

```
# 更新所有依赖
composer update
# 更新指定的包
composer upddate monolog/monolog
# 更新指定的多个包
composer update monolog/monolog symfony/dependency-injection
# 通过通配符匹配包
composer update monolog/monolog symfony/*
```

更新版本受版本约束的约束,一些其他常用命令

```
# 移除一个包及其依赖
composer remove monolog/monolog
```

进行包的搜索,可以在[https://packagist.org/ ](https://packagist.org/中查找需要的包,也可以使用命令行搜索)中查找需要的包,也可以使用命令行搜索

```
composer search monolog
# 仅以名称匹配
composer search --only-name monolog
```




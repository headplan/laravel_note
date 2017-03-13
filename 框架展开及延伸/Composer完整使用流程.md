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

进行包的搜索,可以在 [https://packagist.org/ ](https://packagist.org/中查找需要的包,也可以使用命令行搜索)中查找需要的包,也可以使用命令行搜索

```
composer search monolog
# 仅以名称匹配
composer search --only-name monolog
```

列出项目目前安装的包信息

```
# 列出所有已经安装的包
composer show
# 可以通过通配符进行筛选
composer show monolog/*
# 显示具体某个包的信息
composer show monolog/monolog
```

**版本约束**

确定的版本号

```
{
    "require": {
        "monolog/monolog": "1.19"
    }
}
# 或者
composer require monolog/monolog:1.19
composer require monolog/monolog=1.19
composer require monolog/monolog 1.19
```

指定版本号范围

```
操作符包括： > ， >= ， < ， <= ， != 
使用空格或逗号表示逻辑与,使用||表示逻辑或(与的优先级大于或)
```

> 连字符"-"类似于逻辑与,区别在于连字符右边的版本约束不是确定版本号时,会自动匹配通配符版本1.2会变成1.2.\*

通配符匹配最大版本

`1.0.*`相当于`>=1.0 <1.1`

**下一个重要版本操作符**

~号和^号

```
~ 操作符的用法： ~1.2 相当于 >=1.2 <2.0.0 ，而 ~1.2.3 相当于 >=1.2.3 <1.3.0
^ 操作符的用法： ^1.2.3 相当于 >=1.2.3 <2.0.0 (表示了当前版本到下一版本之间),再例如 ^0.3 会被当作 >=0.3.0 <0.4.0 对待
```






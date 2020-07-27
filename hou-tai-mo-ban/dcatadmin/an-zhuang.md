# 安装

#### 环境

* PHP &gt;= 7.1
* Laravel 5.5.0 ~ 7.\*
* Fileinfo PHP Extension

> 更换阿里云镜像
>
> ```
> composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
> ```

#### 安装Laravel

```
composer create-project --prefer-dist laravel/laravel 项目名称
```

安装完`laravel`之后需要设置数据库连接设置正确 . 

```
composer require dcat/laravel-admin
```

然后运行下面的命令来发布资源 : 

```
php artisan admin:publish
```




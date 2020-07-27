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

在该命令会生成配置文`config/admin.php` , 可以在里面修改安装的地址、数据库连接、以及表名 , 建议都是用默认配置不修改 .

> 执行这一步命令可能会报以下错误`Specified key was too long ... 767 bytes`，如果出现这个报错 , 请在`app/Providers/AppServiceProvider.php`文件的boot方法中加上代码`\Schema::defaultStringLength(191);`，然后删除掉数据库中的所有数据表 , 再重新运行一遍`php artisan admin:install`命令即可 .

启动服务后 , 在浏览器打开`http://localhost/admin/`, 使用用户名`admin`和密码`admin`登陆 . 


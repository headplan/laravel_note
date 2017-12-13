# Laravel-permission

[https://github.com/spatie/laravel-permission](https://github.com/spatie/laravel-permission)

> 包用于Laravel5.4或更高多版本 , 旧版可以查看v1分支 .

```
composer require spatie/laravel-permission
```

Laravel5.5的服务提供者已经可以自动注册了 , 5.4版本可以添加配置 :

```
'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];
```

然后 , 发布迁移文件 :

```
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="migrations"
```

创建表 :

```
php artisan migrate
```

生成配置文件 :

```
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="config"
```

---

两篇关于使用Laravel-permission的文章 :

```
https://scotch.io/tutorials/user-authorization-in-laravel-54-with-spatie-laravel-permission
http://www.qcode.in/easy-roles-and-permissions-in-laravel-5-4/
```

使用Laravel-permission的后台库 : 

```
https://github.com/LaravelDaily/laravel-roles-permissions-manager
```




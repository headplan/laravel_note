# Laravel5.1基础内容

#### 安装

```
composer create-project laravel/laravel Learning_Laravel 5.1.33
```

##### 运行方式

使用PHP内置服务器

```
php -S localhost:8888 -t public
```

使用Laravel artisan启动服务\(其实就是运行了上面的命令,但默认端口为8000\)

```
php artisan serve
```

#### 基本工作流程

##### 操作路由

```php
# /app/Http/routes.php
Route::get('/about', function() {
    return 'Hello World';
});
```




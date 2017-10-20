# 安装与配置

要安装这个包 , 你需要 :

* Laravel 5.1+ or Lumen 5.1+
* PHP 5.5.9+

新版本的需要PHP ^7.0

编辑composer.json文件

```
"require": {
    "dingo/api": "1.0.*@dev"
}

# 新版本
"require": {
    "dingo/api": "2.0.0-alpha1"
}
```

命令行可以使用 :

```
composer require dingo/api:1.0.x@dev
```

> 1.0的版本都属于开发版 , 还没有 **stable** 版本 , 可能需要设置你的 minimum-stability 为 dev .

#### Laravel

打开config/app.php文件 , 注册必要的 service provider  :

```php
'providers' => [ 
    Dingo\Api\Provider\LaravelServiceProvider::class
]
```

然后运行命令 , 会在config中生成api.php配置文件 : 

```php
php artisan vendor:publish --provider="Dingo\Api\Provider\LaravelServiceProvider"
```

> **Lumen**
>
> 打开 bootstrap/app.php , 注册必要的 service providers
>
> ```
> $app->register(Dingo\Api\Provider\LumenServiceProvider::class);
> ```

#### Facades

这个包提供了两个 facades . 可以随意添加任何一个

```
# 这是一个用于api调度的facade,它也为这个包的其他方法提供辅助方法
Dingo\Api\Facade\API
# 这是一个用于API路由的facade,可以用作获取当前路由,请求,检查当前路由名称等
Dingo\Api\Facade\Route
```




# Security\_Passport\_OAuth

Laravel 项目中可以使用 Passport 轻而易举地实现 API 授权过程 , Passport 基于 [League OAuth2 server](https://github.com/thephpleague/oauth2-server) 实现 , 该项目的维护人是[Alex Bilbie](https://github.com/alexbilbie) .

#### 安装

```
composer require laravel/passport
```

将 Passport 的服务提供者注册到配置文件`config/app.php`的`providers`数组中 : 

```
Laravel\Passport\PassportServiceProvider::class,
```

前面的内容完成后 , 运行迁移命令 , 会自动创建应用程序需要的客户端数据表和令牌数据表 : 

```
php artisan migrate
```

> 如果不打算使用 Passport 的默认迁移 , 应该在`AppServiceProvider`的`register`方法中调用`Passport :: ignoreMigrations`方法 , 可以导出这个默认迁移用
>
> `php artisan vendor:publish --tag=passport-migrations`
>
> 命令
>
> ```
> # AppServiceProvider
> use Laravel\Passport\Passport;
> public function register()
> {
>     Passport::ignoreMigrations();
> }
> ```




# User模块

进入Tinker , 查看一下User模型的数据 : 

```
php artisan tinker
>>> App\Models\User::first();
```

创建User资源控制器 : 

```
php artisan make:controller UserController --resource 
```

添加资源路由 , 遵从 RESTful 架构 : 

```
Route::resource('user', 'UserController');
```




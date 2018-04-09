# 用户注册

前面已经创建了用户表 , 现在创建一个用户 :

```
php artisan tinker
>>> use App\Models\User
>>> User::create([
... 'name'=> 'Lartian', 'email'=>'lartisan@lartisan.cn','password'=>bcrypt('123456')
... ]);
```

一些查找方法 :

```
>>> User::find(1)
>>> User::findOrFail(1)
>>> User::first(1)
>>> User::all(1)
```

定义用户资源路由\(routes/web.php\) :

```
Route::resource('users', 'UsersController');
```

查看路由

```
php artisan route:list
```

添加路由方法 : 

```

```




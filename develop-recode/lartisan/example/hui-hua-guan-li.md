# 会话管理

目的 : 由于 HTTP 协议是无状态的 , 无法在两个页面之间保证用户身份的同步 , 因此需要借助会话在浏览器中临时存储用户的身份信息 , 进而保证在同一浏览器中 , 用户在不同页面具有相同的登录状态 .

#### **会话控制器**

```
php artisan make:controller SessionsController
```

然后添加路由

```
Route::get('login', 'SessionController@create')->name('login');
Route::post('login', 'SessionController@store')->name('login');
Route::get('logout', 'SessionController@destroy')->name('logout');
```

路由方法对应添加 . 

**登录表单**

```

```




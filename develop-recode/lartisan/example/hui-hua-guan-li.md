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

```php
<div class="col-md-offset-2 col-md-8">
    <div class="panel panel-default">
        <div class="panel-heading">
            <h5>登录</h5>
        </div>
        <div class="panel-body">
            @include('includes.errors')

            <form method="POST" action="{{ route('login') }}">
                {{ csrf_field() }}

                <div class="form-group">
                    <label for="email">邮箱：</label>
                    <input type="text" name="email" class="form-control" value="{{ old('email') }}">
                </div>

                <div class="form-group">
                    <label for="password">密码：</label>
                    <input type="password" name="password" class="form-control" value="{{ old('password') }}">
                </div>

                <button type="submit" class="btn btn-primary">登录</button>
            </form>

            <hr>

            <p>还没有账号?<a href="{{ route('signup') }}">马上注册</a></p>
        </div>
    </div>
</div>
```

**认证用户**

这里需要些一下store方法

对用户post提交来的数据进行认证 , 这里的验证相对简单 , 只验证了不为空和基本格式的正确与否 :  

```php
$data = $this->validate($request, [
    'email' => 'required|email|max:255',
    'password' => 'required'
]);
```

用户身份认证

借助Laravel中`Auth`的`attempt`方法 , 可以很方便的认证用户数据 : 

```
if (Auth::attempt($data)) {
    # 登录成功
} else {
    # 登录失败
}
```




# 用户注册

前面已经创建了用户表 , 现在创建一个用户 :

```php
php artisan tinker
>>> use App\Models\User
>>> User::create([
... 'name'=> 'Lartian', 'email'=>'lartisan@lartisan.cn','password'=>bcrypt('123456')
... ]);
```

一些查找方法 :

```php
>>> User::findOrFail(1)
>>> User::first(1)
>>> User::all(1)
```

定义用户资源路由\(routes/web.php\) :

```php
Route::resource('users', 'UsersController');
```

查看路由

```php
php artisan route:list
```

这里遵从隐性路由模型绑定的原则 :

> Laravel 会自动解析定义在控制器方法（变量名匹配路由片段）中的 Eloquent 模型类型声明。在上面代码中，由于`show()`方法传参时声明了类型 —— Eloquent 模型`User`，对应的变量名`$user`会匹配路由片段中的`{user}`，这样，Laravel 会自动注入与请求 URI 中传入的 ID 对应的用户模型实例。

实际的路由是这样写的 :

```php
Route::get('/users/{user}', 'UsersController@show')->name('users.show');
```

前面的资源路由已经包含了 . 控制器方法传参中必须包含对应的 Eloquent 模型类型声明 , 并且是有序的 :

```php
public function show(User $user)
{
    return view('users.show', compact('user'));
}
```

当请求[http://lartisan.example/users/1](http://sample.test/users/1)并且满足以上两个条件时 , Laravel 将会自动查找 ID 为 1 的用户并赋值到变量`$user`中 , 如果数据库中找不到对应的模型实例 , 会自动生成 HTTP 404 响应 .

#### Gravatar 头像和侧边栏

在用户模型中定义头像方法

```php
/**
 * md5加密的邮箱地址即可获取Gravatar头像
 * @param int $size
 * @return string
 */
public function gravatar($size = 100)
{
    $hash = md5(strtolower(trim($this->attributes['email'])));
    return "http://www.gravatar.com/avatar/$hash?s=$size";
}
```

获取头像就可以使用用户对象中的方法了

```php
$user->gravatar();
```

视图部分 , 创建includes视图userinfo , 引入到用户的show模板中 . 这里查看源码 .

#### 注册表单

删除之前tinker添加的数据 , 重置数据库并重新运行所有迁移 .

```
$ php artisan migrate:refresh
```

构建表单页面 , 也就是uesr/create/blade.php页面 .

这里一行是提交连接 , 一行是全局辅助函数`old`帮助我们在 Blade 模板中显示旧输入数据

```
{{ route('users.store') }}
{{ old('name') }}
```

**表单数据验证**

前面提交表单使用了store方法 .

表单提交来的数据都需要验证再入库 , Laravel提供了很多种验证方式 , 最近常见的是validator方法 , 接收两个参数 , 第一个参数是用户输入数据 , 第二个是验证规则 . validator方法是`App\Http\Controllers\Controller`类中的`ValidatesRequests`进行定义的 .

几种验证规则 :

**存在性验证** - `required`来验证用户名是否为空 , 也就是必填的表单 .

**长度验证** - 使用`min`和`max`来限制用户名所填写的最小长度和最大长度 .

**格式验证** - 特殊格式 , 例如email , 不用写正则去验证了 .

**唯一性验证** - 验证用户使用的注册邮箱是否已被它人使用 , 这里是针对于数据表`users`做验证 . 更严谨一些 , 还可以在前端和**用户数据表中**设置唯一性 .

**密码匹配验证** - 使用`confirmed`来进行密码匹配验证

```php
public function store(Request $request)
{
    $this->validate($request, [
        'name' => 'required|min:3|max:50',
        'email' => 'required|email|unique:users|max:255',
        'password' => 'required|min:6|max:50'
    ]);
    return;
}
```

**防跨站请求**

防止CSRF请求 , 为了安全考虑 , 要提供一个令牌 , 这里Laravel已经为我们准备好了 : 

```php
{{ csrf_field() }}
# 生成html
<input type="hidden" name="_token" value="fhcxqT67dNowMoWsAHGGPJOAWJn8x5R5ctSwZrAq">
```




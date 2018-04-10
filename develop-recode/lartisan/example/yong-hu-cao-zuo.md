# 用户操作

**更新用户**

前面用户展示已经使用了resource资源路由 , 不用添加编辑路由了

```php
Route::get('/users/{user}/edit', 'UsersController@edit')->name('users.edit');
```

这里也使用了隐性路由模型绑定 , 控制器直接写即可

```php
public function edit(User $user)
{
    return view('users.edit', compact('user'));
}
```

添加edit模板 , 几个注意的点 : 

```
<form method="POST" action="{{ route('users.update', $user->id )}}">
# 转换后为
<form method="POST" action="http://lartisan.example/users/1">
# 伪造的更新资源请求,隐藏域
{{ method_field('PATCH') }}
# 转换后
<input type="hidden" name="_method" value="PATCH">
# 禁止修改的表单
<input type="text" name="email" class="form-control" value="{{ $user->email }}" disabled>
```

修改app.scss , 添加点样式 , 然后修改header头的入口链接 . 

添加update控制器方法 , 基本的逻辑是 : 

首先validate验证表单数据格式 , 验证成功后 , 更新数据 .

 这里的密码部分如果没有修改 , 也就是当用户提供空白密码时 , 赋值原来的密码 , 然后闪存消息提示状态 : 

```php
public function update(User $user, Request $request)
{
    $this->validate($request, [
        'name' => 'required|min:3|max:50',
        'password' => 'nullable|min:6|max:255|confirmed'
    ]);

    $data = [];
    $data['name'] = $request->name;
    if ($request->password) {
        $data['password'] = bcrypt($request->password);
    }
    $user->update($data);

    session()->flash('success', '个人资料更新成功!');
    return redirect()->route('users.show', $user->id);
}
```

#### 操作权限

现在面临的两个问题是 , 未登录状态也可以访问edit和update两个操作 , 登录用户也可以操作别人的个人信息 . 

**中间件过滤**

Laravel 中间件 \(Middleware\)提供了一种非常棒的过滤机制来过滤进入应用的 HTTP 请求 , 中间件文件都存放在`app/Http/Middleware`文件夹中 , Laravel也内置了很多中间件 , 身份验证、CSRF 保护等 . 这里我们使用 Auth 中间件来验证用户的身份时 , 如果用户未通过身份验证 , 则 Auth 中间件会把用户重定向到登录页面 , 通过了继续往下进行 . 

$this-&gt;middleware\(\)的两个参数 , 分别是要使用的中间件和要过滤的操作 , 可以使用except添加黑名单 , 也可以使用其相反的白名单机制only . 

```php
public function __construct()
{
    $this->middleware('auth', [
        'except' => ['show', 'create', 'store']
    ]);
}
```




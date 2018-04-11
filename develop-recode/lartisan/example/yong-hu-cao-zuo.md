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

**授权策略Policy**

使用授权策略 \(Policy\)来对用户的操作权限进行验证 , 在用户未经授权进行操作时将返回 403 禁止访问的异常 , 让用户只能编辑自己的资料 . 创建策略 :

```
php artisan make:policy UserPolicy
```

生成的策略文件在`app/Policies`文件夹下 , 为其添加方法用于用户更新时的权限验证 :

```php
/**
 * id相同表示为相同用户,用户通过授权,可以接着进行下一个操作.
 * @param \App\Models\User $currentUser 当前登录用户实例
 * @param \App\Models\User $user 要进行授权的用户实例
 * @return bool
 */
public function update(User $currentUser, User $user)
{
    return $currentUser->id === $user->id;
}
```

使用授权策略需要注意 :

* 并不需要检查`$currentUser`是不是 NULL . 未登录用户 , 框架会自动为其**所有权限**返回 . 
* 调用时也**不需要**传递当前登录用户至该方法内 , 框架会自动加载当前登录用户 . 

接下来 , 还需要在`AuthServiceProvider`类中对授权策略进行设置 , 其中包含了`policies`属性 , 用于将各种模型对应到管理它们的授权策略上 . 现在我们要做的就是为用户模型`User`指定授权策略`UserPolicy`.

```php
protected $policies = [
    'App\Model' => 'App\Policies\ModelPolicy',
    \App\Models\User::class => \App\Policies\UserPolicy::class,
];
```

定义了授权策略之后 , 就可以在用户控制器中使用`authorize`方法来验证用户授权策略 . `authorize`方法接收两个参数 , 第一个为授权策略的名称 , 第二个为进行授权验证的数据 .

> 默认的`App\Http\Controllers\Controller`类包含了 Laravel 的`AuthorizesRequests`trait . 此 trait 提供了`authorize`方法 , 它可以被用于快速授权一个指定的行为 , 当无权限运行该行为时会抛出 HttpException .

```php
$this->authorize('update', $user);
```

> 这里`update`是指授权类里的`update`授权方法 , `$user`对应传参`update`授权方法的第二个参数 . 正如上面定义`update`授权方法时候提起的 , 调用时 , 默认情况下 , **不需要**传递第一个参数 , 也就是当前登录用户至该方法内 , 因为框架会自动加载当前登录用户 .

接下来将上面的代码添加到需要授权的位置 , 也就是UserController的edit和update方法中 . 

```
public function edit(User $user)
{
    $this->authorize('update', $user);
    return view('users.edit', compact('user'));
}
...
```




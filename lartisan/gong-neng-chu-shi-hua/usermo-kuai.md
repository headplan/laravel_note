# User模块

进入Tinker , 查看一下User模型的数据 :

```
php artisan tinker
>>> App\Models\User::first();
```

**创建User资源控制器** :

```
php artisan make:controller UserController --resource
```

**添加资源路由** , 遵从 RESTful 架构 :

```
Route::resource('user', 'UserController');
```

**隐性路由模型绑定**

这里也是约定优于配置设计范式的体现 , 满足条件自动启用 : 

* 路由声明时必须使用 Eloquent 模型的单数小写格式来作为路由片段参数 , 即{user}
* 控制器方法传参中必须包含对应的 Eloquent 模型类型声明 , 并且是有序的 , `public function show(User $user)`

Laravel 将会自动查找参数返回模型 , 查找不到会自动生成 HTTP 404 响应 . 

**创建views/user目录**

新建show.blade.php模板文件 . 更新show方法 : 

```
public function show(User $user)
{
    return view('user.show', compact('user'));
}
```

这里compact\('user'\)直接使用了User $user绑定的模型变量 . 

**使用Gravatar头像**

获取Gravatar头像 , 将Gravatar登录邮箱进行MD5转码 , 然后拼接URL即可 : 

```
public function gravatar($size = '100')
{
    $hash = md5(strtolower(trim($this->attributes['email'])));
    return "http://www.gravatar.com/avatar/$hash?s=$size";
}
```

创建公共模板引入user.show模板 . 




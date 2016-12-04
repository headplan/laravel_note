# 表单和CSRF认证

表单添加CSRF认证,提交到当前页面

```
<form action="#" method="post">
   {{ csrf_field() }}
```

路由使用any匹配get和post

```
Route::any('admin/login', 'Admin\LoginController@login');
```

控制器接受post数据,判断返回页面,获取验证码,对比验证码返回信息

```
# Input::all() - 获取所有post提交
# back()->with() - 返回页面并返回信息,信息会写入session中
$_code = $this->code->get();
if ($input = Input::all()) {
    if (strtoupper($input['code']) != $_code) {
        return back()->with('msg', '验证码错误');
    }
} else {
    return view('admin.login');
}
```

> 这里有一个坑,新版本的5.2.9以后在路由中默认添加了web中间件,此时在group中再次添加web中间件,session会获取不到
> 
> 这里可以使用php artisan route:list查看一下,就知道了.

将验证码信息返回到页面

```
@if(session('msg'))
<p style="color:red">{{ session('msg') }}</p>
@endif
```




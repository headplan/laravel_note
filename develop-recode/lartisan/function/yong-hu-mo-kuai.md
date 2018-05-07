# 用户模块

#### 注册登录

**生成代码**

```
php artisan make:auth
```

`make:auth`会询问我们是否要覆盖`app.blade.php`这里选择no

可以使用`git status`查看文件更改状态 .

**查看新增的路由**

```
php artisan route:list
```

**删除修改生成的多余内容**

```
# 多余的路由绑定
Route::get('/home', 'HomeController@index')->name('home');
# 多余的控制器和模板
$ rm resources/views/home.blade.php
# 控制修改回原来加载的模板
return view('home.index');
# 中间件暂时排除index
->except('index');
```

**修改顶部模板**

```
# 两处路由别名修改
<li><a href="{{ route('login') }}">登录</a></li>
<li><a href="{{ route('register') }}">注册</a></li>

# 然后是添加判断,这里直接判断未登录用户和登录用户展示的页面内容
@guest
    <li><a href="{{ route('login') }}">登录</a></li>
    <li><a href="{{ route('register') }}">注册</a></li>
@else
    .
    .
    .
@endguest

# 还要注意下退出登录时候的CSRF令牌,退出用js事件去提交
onclick="event.preventDefault();document.getElementById('form-logout').submit()
<form id="logout-form" action="{{ route('logout') }}" method="POST" style="display: none;">
    {{ csrf_field() }}
</form>
```

**执行迁移**

```
php artisan migrate
```

现在基本的注册登录已经可以使用 , 修改登录注册中的跳转地址 :

```
protected $redirectTo = '/';
modified:   app/Http/Controllers/Auth/LoginController.php
modified:   app/Http/Controllers/Auth/RegisterController.php
modified:   app/Http/Controllers/Auth/ResetPasswordController.php
modified:   app/Http/Middleware/RedirectIfAuthenticated.php
```

#### **整体模板修改**

```
# 基本上是注册登录逻辑的四个模板
login.blade.php
register.blade.php
email.blade.php
reset.blade.php
```

**login.blade.php**

```
# 修改模板页面
# 主要是路由连接地址和错误提示
{{ route('password.request') }}
{{ $errors->has('password') ? ' is-danger' : '' }}
# 登录状态使用了组件b-checkbox
# 简单引入极验验证,流程查看扩展包使用下属文章,暂时未引入
```

**register.blade.php**

```
# 添加了注册验证码,直接调用开发包,后边JS随机个数切换验证码
<img class="captcha" src="{{ captcha_src('flat') }}" 
    onclick="this.src='/captcha/flat?'+Math.random()" title="点击图片重新获取验证码">
```

重写控制器中对注册的验证规则 :

修改\_app/Http/Controllers/Auth/RegisterController.php中的\_validator方法 , 并添加验证信息 :

```
    'captcha' => 'required|captcha',
], [
    'captcha.required' => '验证码不能为空',
    'captcha.captcha' => '请输入正确的验证码',
]);
```

> 安装Laravel-lang , 之前忘记安装了 .




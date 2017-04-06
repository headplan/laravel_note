# 认证授权代码解析

执行php artisan make:auth命令之后的变动

```
# 路由发生变化 routes/web.php
# 生成了控制器 app/Http/Controllers/HomeController.php
# 生成了下列视图
resources/views/auth/
resources/views/home.blade.php
resources/views/layouts/
```

执行完命令生成的路由routes/web.php

```
# php artisan make:auth
Auth::routes();
# 代码位置illuminate/Routing/Router.php里面的auth()方法
public function auth()
{
    // Authentication Routes...
    $this->get('login', 'Auth\LoginController@showLoginForm')->name('login');
    $this->post('login', 'Auth\LoginController@login');
    $this->post('logout', 'Auth\LoginController@logout')->name('logout');

    // Registration Routes...
    $this->get('register', 'Auth\RegisterController@showRegistrationForm')->name('register');
    $this->post('register', 'Auth\RegisterController@register');

    // Password Reset Routes...
    $this->get('password/reset', 'Auth\ForgotPasswordController@showLinkRequestForm')->name('password.request');
    $this->post('password/email', 'Auth\ForgotPasswordController@sendResetLinkEmail')->name('password.email');
    $this->get('password/reset/{token}', 'Auth\ResetPasswordController@showResetForm')->name('password.reset');
    $this->post('password/reset', 'Auth\ResetPasswordController@reset');
}
```

生成了HomeController.php控制器

```
<?php
# 注入了auth中间件,代表没有通过认证的用户无法访问这个路由控制器

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class HomeController extends Controller
{
    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth');
    }

    /**
     * Show the application dashboard.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        return view('home');
    }
}
```

配置文件config/auth.php

```
# 这里guard的概念可以理解为就是一个角色,现在又web和api两种角色
# 其中的driver的值是session,意思就是表示这个认证要怎么去保存用户状态
# 很明显,web的是session,api的是token
# 还有一个provider的值都是users,这里的配置调用的就是下面的providers数组
# provider就是告诉Laravel你的用户信息保存在哪一张表里面,driver就是告诉了要使用那种方式来操作数据库
'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],

    'api' => [
        'driver' => 'token',
        'provider' => 'users',
    ],
],

# 这里的第一个users里driver指明了驱动方式是eloquent,第二个注释的是database
# model指的是user.php模型
# 第二种直接指明了table表示users
'providers' => [
    'users' => [
        'driver' => 'eloquent',
        'model' => App\User::class,
    ],

    // 'users' => [
    //     'driver' => 'database',
    //     'table' => 'users',
    // ],
],
```

**理解了上面的一些基本概念,再去看看手册上的认证相关的内容**

```
# 四个基本控制器,代表了4个基本的功能
# LoginController 用于处理用户登录认证
# RegisterController 用于处理新用户注册
# ForgotPasswordController 用于处理重置密码邮件链接
# ResetPasswordController 包含重置密码逻辑

# 但是看这4个控制器基本上没写什么逻辑,原因是代码逻辑都写在use的trait里了
# 还有一个共同特点就是,这四个控制器都引入了guest中间件
# 这个中间件是在App\Http\Middleware下的
public function handle($request, Closure $next, $guard = null)
{
    if (Auth::guard($guard)->check()) {
        return redirect('/home');
    }

    return $next($request);
}
# 这个中间件根据guard来check用户是否通过认证,认证通过则跳到/home页
# 这4个控制器中分别包含着不同的trait
# LoginController
    ->AuthenticatesUsers:登录逻辑
        ->RedirectsUsers:跳转逻辑
        ->ThrottlesLogins:登录失败次数限制
# RegisterController
    ->RegistersUsers:注册逻辑
        ->RedirectsUsers:跳转逻辑
# ResetPasswordController:重置密码
    ->ResetsPasswords:充值密码逻辑
    ->RedirectsUsers:跳转逻辑
# ForgotPasswordController:重置密码邮件
    ->SendsPasswordResetEmails:发送重置密码邮件逻辑
```

### 认证

手册中的认证就是对trait的一些覆盖方式.

**自定义路径**

其实包含了RedirectsUsers的Trait都可以自定义路径,控制器中都包含$redirectTo属性,也可以定义一个redirectTo方法,优先级较高

**自定义用户名**

这里就是覆盖Login认证中的逻辑,默认是email认证.

```
public function username()
{
    return 'username';
}
```

**自定义Guard**

前面已经定义了配置文件config/auth.php中定义的guard,如果需要自定义,要在注册,登录,找回密码中覆盖guard函数.

```
protected function guard()
{
    return Auth::guard('guard-name');
}
```

**自定义验证/存储**

这里直接修改注册的控制器即可.

### 获取认证用户

这里有两种方式,一种是通过Auth门面,一种是通过注入的Request对象访问

```
$user = Auth::user();
$request->user();
```

Auth门面还可以直接check用户是否认证Auth::check\(\),但一般情况会通过中间件直接路由保护

```
# 路由保护的两种方式
Route::get('profile', function() {
    // 只有认证用户可以进入...
})->middleware('auth');
public function __construct(){
    $this->middleware('auth');
}
```

使用了路由中间件之后,还需要指定guard来实现认证的.这里写的就是配置文件

```
$this->middleware('auth:api')
```

登录逻辑中还包含了一个登录失败次数限制的trait,在`Illuminate\Foundation\Auth\ThrottlesLogins`中.

以上的都是覆盖框架自带的方法去实现逻辑.其实完全可以自定义手动去认证用户,手动认证就不会显得这么乱了.

### 手动认证用户 {#toc_9}

手动认证,这里就是完全不使用框架自带的控制器,而是去自己自定义控制器,去写登录认证逻辑.

当然,还是需要调用Auth门面来完成认证.

```
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Auth;

class LoginController extends Controller
{
    /**
     * 处理认证尝试.
     *
     * @return Response
     * @translator laravelacademy.org
     */
    public function authenticate()
    {
        if (Auth::attempt(['email' => $email, 'password' => $password])) {
            // 认证通过...
            return redirect()->intended('dashboard');
        }
    }
}
```

Auth::attempt方法第一个参数,就是check接收数据认证一下,通过返回true

redirect\(\)-&gt;intended\('dashboard'\);重定向redirect后的intended方法是返回没认证时候访问的页面.

要验证的内容可以是多样的,比如还可以验证active,username等字段

**访问指定 Guard 实例**

使用Auth::guard\('admin'\)-&gt;attempt\($credentials\)指定配置文件中guard的键,

这种机制允许你在同一个应用中对不同的认证模型或用户表实现完全独立的用户认证.

**退出**直接Auth::logout\(\);即可.

**记住用户逻辑**

直接使用


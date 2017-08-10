# Security\_Auth

快速启动认证系统只需要两个命令

```
php artisan make:auth
php artisan migrate
```

Migrate是创建database/migrations下的迁移 . 当然创建之前 , 要配置数据库连接在.env文件中 . 这里有几点要注意 :

* 创建数据库表结构时 , 确认密码字段最少必须 60 字符长 , 保持255即可
* `users`数据表中必须含有 nullable  , 100 字符长的`remember_token`字段 , 这是给**记住我**这个功能用的

---

#### 配置文件

先来看看认证系统的**配置文件** . 其中包含了用于调整认证服务行为的 , 标注好注释的选项配置 . Laravel 的认证组件由`guards`和`providers`组成的 .

* Guard 定义了用户在每个请求中如何实现认证
* Provider 定义了如何从持久化存储中获取用户信息

再来看看配置文件

```
# 选项控制应用程序的默认身份验证 "保护" 和密码重置选项
'defaults' => [
    'guard' => 'web',
    'passwords' => 'users',
],

# 这里就是定义身份验证保护的选项,也就是定义guard的选项
# 因为一切都是HTTP请求,所以Guard也只支持session和token两种驱动方式.
# 定义了驱动方式,然后需要定义用户提供者provider.所有的认证却动都要有一个用户提供者.
# 这里是key,下面会定义具体的获取检索用户的方式
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

# 用户提供者,也就是认证获取用户数据的源.
# 支持Eloquent和Database两种方式,如果有多个用户表或模型
# 就可以创建多个分别分配到guard上
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

# 重置密码
# 这里和前面provider一样,如果有多个用户表或模型,并且基于不同的用户类型进行不同的设置,可以创建多个
# 选项对应了用户提供者,数据表名,和过期时间(60分钟).
'passwords' => [
    'users' => [
        'provider' => 'users',
        'table' => 'password_resets',
        'expire' => 60,
    ],
],
```

#### 认证系统

Laravel 带有几个预设的认证控制器 , 它们被放置在`App\Http\Controllers\Auth`命名空间内

* RegisterController - 处理用户注册
  * RegistersUsers Trait
    * RedirectsUsers Trait
* LoginController - 处理用户登录认证
  * AuthenticatesUsers Trait
    * RedirectsUsers Trait
    * ThrottlesLogins Trait
* ForgotPasswordController - 处理重置密码的 e-mail 链接
  * SendsPasswordResetEmails Trait
* ResetPasswordController - 包含重置密码的逻辑
  * ResetsPasswords Trait
    * RedirectsUsers Trait

这些控制器使用了 trait 来包含所需要的方法 .

```
php artisan make:auth
```

命令生成所有的认证路由 , layout 布局 , 注册和登录视图 , 同时生成`HomeController`控制器 , 用来处理登录成功后跳转的控制器 .

**路由**

php artisan route:list 查看所有路由![](/assets/routelist.png)**视图**

下面是make:auth命令生成的视图文件 , tree显示的很清晰 , 除了原来的welcome.blade.php

```
./resources/views
├── auth/
│   ├── login.blade.php
│   ├── passwords/
│   │   ├── email.blade.php
│   │   └── reset.blade.php
│   └── register.blade.php
├── home.blade.php
├── layouts/
│   └── app.blade.php
└── welcome.blade.php
```

**认证**

下面看看Laravel自带的Auth类 .

**自定义路径**

用户成功登录 , 默认跳转/home路由 . 可以通过在

* `LoginController`
* `RegisterController`
* `ResetPasswordController`

中设置`redirectTo`属性来自定义登录认证成功之后的跳转路径 . 这三个类中都使用了RedirectsUsers Trait . 其中定义了redirectTo使用的逻辑 . 如果跳转路径需要自定义逻辑来生成 , 可以定义`redirectTo()`方法来代替redirectTo属性 . 看一下RedirectsUsers Trait中的redirectPath方法 , 定义`redirectTo()`方法优先级高于redirectTo属性 .

**自定义用户名**

Laravel默认使用`email`字段来认证 , 定义在LoginController中使用的AuthenticatesUsers Trait中的username\(\)方法 , 可以在LoginController中重写username\(\)方法 , return要验证的登录字段 .

**自定义Guard**

配置文件中的default中定义了web为默认的guard , 定义了自己的认证guard , 在Auth认证系统中使用 , 要在

* `LoginController`
* `RegisterController`
* `ResetPasswordController`

三个控制前中重写guard\(\)方法 , 因为他们使用的trait中默认使用了default的web的guard . 重写即可

```
protected function guard()
{
    return Auth::guard('guard-name');
}
```

**自定义验证 / 存储**

用户的注册在RegisterController类中完成 , 应用验证输入参数和创建新用户也在其中定义 .

* `validator`方法包含了新用户的验证规则
* `create`方法负责使用 Eloquent ORM 在数据库中创建新的 App\User 记录

**获取已认证的用户**

**Request实例**

```
public function update(Request $request)
{
    // $request->user() 返回认证过的用户的实例...
}
```

**Auth facade**

* Auth::user\(\) - 获取当前已通过认证的用户
* Auth::id\(\) - 获取当前已通过认证的用户id
* Auth::check\(\) - 检查用户是否登录

在这里的check\(\)方法之前 , 还可以使用中间件检查用户是否认证过 .

**认证中间件**

在 HTTP kernel 中注册了Illuminate\Auth\Middleware\Authenticate中间件 , 可以在路由中直接使用 , 也可以在控制器初始化时指定 .

* middleware\('auth'\);

还可以直接指定guard

* middleware\('auth:api'\);
* middleware\('auth',\['web','api'\]\);

**登录限流**

这个功能在ThrottlesLogins Trait中实现 , 用户在进行几次尝试后仍不能提供正确的凭证 , 将在一分钟内无法进行登录 .

定义属性 , 控制次数与时间

* maxAttempts
* decayMinutes

限制针对

* $this-&gt;username\(\)
* $request-&gt;ip\(\)

#### 手动认证用户

如果不使用Laravel内置的认证控制器 , 可以使用**Auth Facade**来访问Laravel认证服务 , 即`use Illuminate\Support\Facades\Auth;`

`Auth::attempt()` - 接受一个数组来作为第一个参数 , 可用来寻找数据库里的用户数据 , 数组参数可以添加用户名,密码,或其他状态 . 第二个参数是bool值 , 判断是否remember , 前提是要认证的用户表中必须要包含remember\_token字段 , 就可以快速实现记住我功能 .

是否记住我 - `Auth::viaRemember()`判断是否使用记住我cookie来做认证 .

除了config/auth.php配置中默认的Guard , 还可以手动的指定Guard , 应用不同部分管理用户认证时使用完全不同的认证模型或者用户表 :

```
if (Auth::guard('admin')->attempt($credentials)) {
    //
}
```

注销用户 - `Auth::logout()`这个方法会清除所有认证后加入到用户 session 的数据 .

#### 其他认证方法

* Auth::login\($user, true\) - 用用户实例做认证 , 第二个参数为记住用户 . 
* Auth::guard\('admin'\)-&gt;login\($user\) - 指定guard
* Auth::loginUsingId\(1, true\) - 登录指定 ID 用户 , 也可以是要登录用户的主键 , 第二个参数为记住用户 . 
* Auth::once\($credentials\) - 针对一次性认证用户 , 没有任何的 session 或 cookie 会被使用 . 参数和attempt\(\)一样 . 

#### HTTP基础认证

auth.basic中间件被增加到路由上 , 当使用浏览器进入这个路由时 , 将自动的被提示需要提供凭证 . 它不需要登录页面 ,

```
Route::get('profile', function () {
    // 只有认证过的用户可进入...
})->middleware('auth.basic');
```

> 使用FastCGI , 肯能会让基础认证无法运行 , 在public的.htaccess文件中加入下面几行即可
>
> ```
> RewriteCond %{HTTP:Authorization} ^(.+)$
> RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
> ```

#### 无状态 HTTP 基础认证

onceBasic和上面的Auth::once的功能一样 , 没有任何的 session 或 cookie 会被使用 , 一次性的 , 默认参数验证字段email . 更方便的方法是 , 新建中间件 , 调用Auth::onceBasic方法 , 然后在路由或控制器中直接使用 .

```
public function handle($request, $next)
{
    return Auth::onceBasic() ?: $next($request);
}
```

上面提到的手动认证或者HTTP认证 , 无状态等方法都来自Illuminate\Auth\SessionGuard , 它实现了两个接口 , StatefulGuard和SupportsBasicAuth . StatefulGuard是前面提到的方法 , SupportsBasicAuth中的就是Basic和onceBasic两个方法 .

### 自定义Guard和用户提供者

稍后更新...

---

> Learning\_Laravel的测试代码中 , 添加了多用户的控制 , 下面的内容都会根据原Auth命令生成的基础上修改添加 . 为了让测试代码更加有针对性 , 上面的各种认证方式都会在不同的路由中测试 .

在创建auth之前 , 新建一个迁移表`php artisan make:migration create_admins_table --create=admins`

```
Schema::create('admins', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();
    $table->string('title'); # 比Users多了个字段,记录称谓
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
```

然后创建一个Model , 复制User.php的内容`php artisan make:model Admin`

分析一下User.php

```
<?php
# 命名空间
namespace App;
# 这里用到了消息通知,下面有use Notifiable,关于消息通知查看下面的内容
use Illuminate\Notifications\Notifiable;
# 这个是User的Base Model,它也是继承Model的,也叫User.只是起了别名Authenticatable,其继承了3个接口,然后分3个trait完成了接口的功能
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * The attributes that are mass assignable.
     * 表示可以被复制的字段,Eloquent中有解释
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     * 表示可以隐藏的字段
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];
}
```

* 接下来配置新的Guard和Provider , 具体查看实例中的config/auth.php配置
* 创建admin路由和控制器以及视图 . 
* commit一次

**接下来创建控制器**

* AdminLoginController - 先来看看LoginController这个控制器的实体 , 在use AuthenticatesUsers中 , 前面已经提到了.

  * 路由访问的是showLoginForm方法和login方法
  * 所以创建Admin的登录控制器`php artisan make:controller Auth/AdminLoginController`
  * 创建上面两个方法showLoginForm和login , 先完成show方法展示页面 . 提交action绑定到login方法

  * 初始化配置中间件`guest:admin`

  * 绑定路由

  * 再来看看login方法的步骤
    * 验证form表单数据 , 也就是email和password字段
    * 尝试登陆 , 这里用到了前面的自定义认证`Auth::guard('admin')->attempt($credentials, $remember)`
      * 其中password字段被排除了trim过滤 , 可以查看trim全局中间件
    * 登陆成功重定向跳转 , 这里使用了intended\(\)方法 , 表示将用户请求重定向至被用户验证过滤器拦截之前用户试图访问URL中去 . 
    * 认证失败重定向到上一页并带着提交的数据
    * 更多逻辑后续添加
  * 使用tinker创建admin数据

---

#### 




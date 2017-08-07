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
* LoginController - 处理用户登录认证
* ForgotPasswordController - 处理重置密码的 e-mail 链接
* ResetPasswordController - 包含重置密码的逻辑

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
│   ├── login.blade.php
│   ├── passwords/
│   │   ├── email.blade.php
│   │   └── reset.blade.php
│   └── register.blade.php
├── home.blade.php
├── layouts/
│   └── app.blade.php
└── welcome.blade.php
```

---

> Learning\_Laravel的测试代码中 , 添加了多用户的控制 , 下面的内容都会根据原Auth命令生成的基础上修改添加 .

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
# 这里用到了消息通知,下面有use Notifiable
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

---

#### 




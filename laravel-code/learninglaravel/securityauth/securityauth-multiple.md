# Security\_Auth\_Multiple

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
  * 使用tinker创建admin数据 , 登录成功
  * 重写logout , 先新建一个component视图 , 区分logout的是user还是admin .
    * 默认的login控制器自带了一个logout , 但是会清空所有的session记录 , 这里分别重写
    * logout和userLogout , 添加路由
    * 分别在控制器的guest中间件中排除掉logout方法

**跳转重定向的问题**

前面的内容已经基本完成了一个手动的验证流程 , 但是有跳转的问题 , 比如登录了admin , 再访问admin/login会跳转到login页面 , 原因是laravel的中间件两处硬编码写死的跳转 .

第一个在Laravel的异常中 , app/Exceptions/Handler.php , 其中的unauthenticated方法 , 将身份验证异常转换为未经身份验证的响应 , 也就是最后面写的跳转到登录页面 , 这里写死了跳转的地址 . 修改为

```php
protected function unauthenticated($request, AuthenticationException $exception)
{
    if ($request->expectsJson()) {
        return response()->json(['error' => 'Unauthenticated.'], 401);
    }

    $guard = array_get($exception->guards(), 0);
    switch ($guard) {
        case 'admin':
            $login = 'admin.login';
            break;
        default:
            $login = 'login';
            break;
    }

    return redirect()->guest(route($guard));
}
```

另一处在RedirectIfAuthenticated中间件里 , 这里check就是前面提到的在中间件中检查用户是否登录 , 如果登录跳转的页面也是写死的 , 稍作修改

```php
public function handle($request, Closure $next, $guard = null)
{
    switch ($guard) {
        case 'admin':
            if (Auth::guard($guard)->check()) {
                return redirect()->route('admin.dashboard');
            }
            break;
        default:
            if (Auth::guard($guard)->check()) {
                return redirect('/home');
            }
            break;
    }

    return $next($request);
}
```

**忘记密码功能**  
查看一下自带的路由  
![](/assets/passwordroute.png)

* 路由password/reset , 访问ForgotPasswordController控制器showLinkRequestForm方法 , 展示视图email.blade.php
* 路由password/email , 根据post得到的邮件 , 发送消息ResetPasswordNotification . 访问ForgotPasswordController控制器的sendResetLinkEmail方法
* 路由password/reset/{token} , 访问重置密码链接 , ResetPasswordController控制器的showResetForm方法 , 展示视图reset.blade.php
* 路由password/reset , 操作数据重置密码 . ResetPasswordController控制器的reset方法

新建这两个控制器 , AdminForgotPasswordController和AdminResetPasswordController . 新建Admin相关的重置密码路由 .

**AdminForgotPasswordController**

* use Illuminate\Support\Facades\Password
* 中间件使用guard , 即guest:admin
* password broker - 这里就是我们在auth.php配置文件中配置的passwords重置密码配置 . 
* 重写broker方法 , 选择admins , Password::broker\('admins'\)
* 重写showLinkRequestForm方法 , 返回要展示的admin的忘记密码视图 , 这里绑定的是password/reset路由
* 新建Admin视图 - auth.passwords.email-admin
* 控制器中还给sendResetLinkEmail方法绑定了password/email的post路由 , 先看一下trait种原来自带的 : 
  1. 验证数据 - $this-&gt;validateEmail\($request\);
  2. 向该用户邮箱发送密码重置的链接 , 尝试发送链接并检查响应 , 然后向用户显示消息 , 最后返回响应 . 
     * 用了sendResetLink方法
     * 在Illuminate\Auth\Passwords\PasswordBroker中 , 是PasswordBrokerContract的实现
     * 首先检查在给定的凭据中\(这里就是email\) , 是否找到了一个用户，如果没有将用会话中的一个“flash”数据重定向到当前URI , 以向开发人员指出错误 . 
       * 就是$user = $this-&gt;getUser\($credentials\);和下面的is\_null\(\)判断 , 这里的$user就是model对象
     * 获取重置令牌 , 然后就可以用一个重置密码的链接将消息发送给这个用户 . 
       * 就是sendPasswordResetNotification\(\)和其中的$this-&gt;tokens-&gt;create\(\) , 就是创建个token , 前面的方法在
         * Illuminate\Auth\Passwords\CanResetPassword这个trait中 , 里面就是执行发送消息的notify\(\)方法 , 里面的对象定义了消息的内容 .  
     * 然后 , 将重定向回当前的URI , 在会话中没有设置任何东西来指示错误 .
       * return static::RESET\_LINK\_SENT
  3. 判断$response == passwords.sent , 执行sendResetLinkResponse方法 . 
* 现在在admin model中重写前面的提到的发送信息的方法sendPasswordResetNotification , 因为发送消息都是通过$user发出去的 . 其中的消息内容类改为AdminResetPasswordNotification , 这个类还不存在 , 创建他 . 
* php artisan make:notification AdminResetPasswordNotification
  * 修改这个消息类 , 初始化$token属性

**AdminResetPasswordController**

* use Illuminate\Http\Request
  use Illuminate\Support\Facades\Password
  use Illuminate\Support\Facades\Auth
* 因为这个类是重置密码 , 用到了Auth , 然后修改成功后跳转redirectTo , 和中间件使用的guard , 即guest:admin
* password broker - 这里就是我们在auth.php配置文件中配置的passwords重置密码配置 . 
* 重写broker方法 , 选择admins , Password::broker\('admins'\)
* 重写guard方法 , 选择admin , Auth::guard\('admin'\) , 因为有调用guard\(\)的地方 .
* 重写showResetForm方法 , 返回要展示的admin的忘记密码视图 , 这里绑定的是password/reset/{token}路由
* 新建Admin视图 - auth.passwords.reset-admin
* 最后一步就是执行reset , 把修改的密码入库 , 因为前面重写的方法已经定义了guard和broker , 随意这里就不用修改reset方法了 . 

最后配置mailtrap.io , 测试邮件发送 , 修改密码 . 






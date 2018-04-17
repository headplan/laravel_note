# 邮件发送

#### 账户激活

账号激活功能只有当用户成功激活自己的账号时才能在网站上进行登录 , 这里需要为用户表新增两个字段用于保存用户的激活令牌和激活状态 . 用于验证用户身份和判断用户是否已激活 .

**激活流程**

* 用户注册成功后 , 自动生成激活令牌
* 将激活令牌以链接的形式附带在注册邮件里面 , 并将邮件发送到用户的注册邮箱上
* 用户点击注册链接跳到指定路由 , 路由收到激活令牌参数后映射给相关控制器动作处理
* 控制器拿到激活令牌并进行验证 , 验证通过后对该用户进行激活 , 并将其激活状态设置为已激活
* 用户激活成功 , 自动登录

**添加字段**

```
php artisan make:migration add_activation_to_users_table --table=users
```

运行迁移 , 加入字段

```
php artisan migrate
```

**生成令牌**

用户的激活令牌需要在用户创建\(注册\)之前就先生成好 , 这里用到了Eloquent模型默认提供的事件 , 这些事件可以用来监听到模型的创建,更新,删除,保存等操作 . 例如 :

* `creating`用于监听模型被创建之前的事件
* `created`用于监听模型被创建之后的事件

```php
public static function boot()
{
    parent::boot();

    static::creating(function ($user) {
        $user->activation_token = str_random(30);
    });
}
```

`boot`方法会在用户模型类完成初始化之后进行加载 , 这里使用了`creating`方法用于监听模型被创建之前的事件 , 也就是生成token .

然后更新模型工厂 , 并将生成的假用户和第一位用户都设为已激活状态 :

```php
# 模型工厂
'activated' => true,
# 第一位用户
$user->activated = true;
```

重置并填充数据 :

```
$ php artisan migrate:refresh --seed
```

**邮件程序**

这里暂时使用log邮件驱动的方式来调试邮件发送功能 , 可以在`storage/logs/laravel.log`文件中查看调试 :

```
MAIL_DRIVER=log
```

**定义路由**

```php
Route::get('signup/confirm/{token}', 'UsersController@confirmEmail')->name('confirm_email');
```

**创建邮件模板**

```
resources/views/emails/confirm.blade.php
```

**登录时检查是否已激活**

```php
$user = Auth::user();
if ($user->activated) {
    session()->flash('success', "欢迎回来,{$user->name}!");
    return redirect()->intended(route('users.show', [$user]));
} else {
    Auth::logout();
    session()->flash('warning', '你的账号未激活,请检查邮箱中的注册邮件进行激活.');
    return redirect('home');
}
```

#### 发送邮件

通过`Mail`接口的`send`方法来进行邮件发送 , 在控制器中创建邮件发送方法 :

```php
protected function sendEmailConfirmationTo($user)
{
    # 视图文件名
    $view = 'emails.confirm';
    # 传入的用户数据
    $data = compact('user');
    # 发送的邮箱
    $from = 'headplan@163.com';
    # 发送者
    $name = 'Lartisan';
    # 接收者
    $to = $user->email;
    # 发送内容
    $subject = '感谢注册 Lartisan 应用!请确认你的邮箱!';

    # 发送邮件
    Mail::send($view, $data, function ($msg) use ($from, $name, $to, $subject) {
        $msg->from($from, $name)->to($to)->subject($subject);
    });
}
```

现在要修改注册成功的登录动作改为发送激活邮件 , 修改闪存信息 , 然后重定向首页 :

```php
$this->sendEmailConfirmationTo($user);
session()->flash('success', '验证邮件已发送到你的注册邮箱上,请注意查收.');
return redirect('home');
```

#### 激活功能

邮件已经发送出去 , 接下来要完成激活的操作 , 并在中间件开启未登录用户访问权限 :

```php
$this->middleware('auth', [
    'except' => ['show', 'create', 'store', 'index', 'confirmEmail']
]);
```

```php
public function confirmEmail($token)
{
    $user = User::where('activation_token', $token)->firstOrFail();

    $user->activated = true;
    $user->activation_token = null;
    $user->save();

    Auth::login($user);
    session()->flash('success', '恭喜你,激活成功!');
    return redirect()->route('users.show', [$user]);
}
```

测试一下功能 , 可以追一下log

```
tail -f storage/logs/laravel.log
```

#### 密码重设

**操作流程**

* 用户点击重设密码链接并跳转到重设密码页面
* 在重设密码页面输入邮箱信息并提交
* 控制器通过该邮箱查找到指定用户并为该用户生成一个密码令牌 , 接着将该令牌以链接的形式发送到用户提交的邮箱上
* 用户查看自己个人邮箱 , 点击重置密码链接跳转到重置密码页
* 用户在该页面输入自己的邮箱和密码并提交
* 控制器对用户的邮箱和密码重置令牌进行匹配 , 匹配成功则更新用户密码

**数据表**

默认生成的密码重置表有三个字段 email, token, created\_at , 分别用于生成用户邮箱、密码重置令牌、密码重置令牌的创建时间 , 并为邮箱和密码重置令牌加上了索引 .

**创建路由**

密码重设功能相关的逻辑代码都放在了 ForgotPasswordController 和 ResetPasswordController 了 .

```php
# 显示重置密码的邮箱发送页面
Route::get('password/reset', 'Auth\ForgotPasswordController@showLinkRequestForm')->name('password.request');
# 邮件发送重置链接
Route::post('password/email', 'Auth\ForgotPasswordController@sendResetLinkEmail')->name('password.email');
# 密码更新页面
Route::get('password/reset/{token}', 'Auth\ResetPasswordController@showResetForm')->name('password.reset');
# 执行密码更新操作
Route::post('password/reset', 'Auth\ResetPasswordController@reset')->name('password.update');
```

**添加重置密码入口到页面**

```php
<a href="{{ route('password.request') }}">忘记密码?</a>
```

**创建重置密码页面**

resources/views/auth/passwords/email.blade.php

```php
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <div class="panel panel-default">
                <div class="panel-heading">重置密码</div>
                <div class="panel-body">
                    @if(session('status'))
                        <div class="alert alert-success">
                            {{ session('status') }}
                        </div>
                    @endif

                    <form class="form-horizontal" method="POST" action="{{ route('password.email') }}">
                        {{ csrf_field() }}

                        <div class="form-group {{ $errors->has('email') ? 'has-error' : '' }}">
                            <label for="email" class="col-md-4 control-label">邮箱地址:</label>

                            <div class="col-md-6">
                                <input id="email" type="text" class="control-label" name="email" value="{{ old('email') }}" required>

                                @if($errors->has('email'))
                                    <span class="help-block">
                                        <strong>{{ $errors->first('email') }}</strong>
                                    </span>
                                @endif
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="col-md-6 col-md-offset-4">
                                <button type="submit" class="btn btn-primary">
                                    发送密码重置邮件
                                </button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
```

**创建更新密码页面**

```
resources/views/auth/passwords/reset.blade.php
```

页面中的令牌通过隐藏域提交给控制器

```php
<input type="hidden" name="token" value="{{ $token }}">
```

修改控制器中修改密码后跳转的属性ResetPasswordController :

```php
protected $redirectTo = '/';
```

测试可以追log查看重置密码的链接

```php
tail -f storage/logs/laravel.log
```

#### 邮件程序

重新定制密码重置邮件功能 . 生成消息通知文件 :

```
$ php artisan make:notification ResetPassword
```

生成`app/Notifications/ResetPassword.php`文件 .

修改这个文件 :

```php
public function __construct($token)
{
    $this->token = $token;
}

public function toMail($notifiable)
{
    return (new MailMessage)
                ->line('这是一封密码重置邮件,如果是您本人操作,请点击以下按钮继续:')
                ->action('重置密码', url(config('app.url').route('password.reset', $this->token, false)))
                ->line('如果您并没有执行此操作,您可以选择忽略此邮件.');
}
```

User模型里调用 :

```php
public function sendPasswordResetNotification($token)
{
    $this->notify(new ResetPassword($token));
}
```

重置密码的Email视图 :

```php
$ php artisan vendor:publish --tag=laravel-notifications
```

测试功能 . 

#### 在生产环境中发送邮件

Laravel 的通知系统默认支持邮件频道的通知方式 , 开箱机用 . 

Laravel 支持多种邮件驱动 , 包括 smtp、Mailgun、Maildrill、Amazon SES、mail 和 sendmail . 其他 , 如Mailgun 、 Amazon SES 、Maildrill 都是第三方邮件服务 . mail 驱动使用 PHP 提供的 mail 函数 . 

> sendmail 驱动通过 Sendmail/Postfix（Linux）提供的命令发送邮件，smtp 驱动使用支持 ESMTP 的 SMTP 服务器发送邮件。mail 不安全，sendmail 需要安装配置 Sendmail/Postfix，并且信用不高，很容易被当成垃圾邮件，第三方服务的信用是最高的，商业软件推荐使用。

**开启SMTP功能**

QQ或者网易邮箱中 , 开启POP3/SMTP服务 . 获取客户端授权码 , 授权码将作为我们的密码使用 . 

**邮箱发送配置**

```
MAIL_DRIVER=smtp
MAIL_HOST=smtp.163.com
MAIL_PORT=25
MAIL_USERNAME=xxxxxxxxxxxxxx@163.com
MAIL_PASSWORD=xxxxxxxxx
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=xxxxxxxxxxxxxx@163.com
MAIL_FROM_NAME=Lartisan
```

* MAIL\_DRIVER=smtp —— 使用支持 ESMTP 的 SMTP 服务器发送邮件
* MAIL\_HOST=smtp.163.com —— 邮箱的 SMTP 服务器地址 , 必须为此值
* MAIL\_PORT=25 —— 邮箱的 SMTP 服务器端口 , 必须为此值
* MAIL\_USERNAME=xxxxxxxxxxxxxx@163.com —— 请将此值换为你的邮箱
* MAIL\_PASSWORD=xxxxxxxxx —— 密码是我们前面拿到的授权码
* MAIL\_ENCRYPTION=tls —— 加密类型 , 选项 null 表示不使用任何加密 , 其他选项还有 ssl , 这里使用 tls 即可
* MAIL\_FROM\_ADDRESS=xxxxxxxxxxxxxx@163.com —— 此值必须同 MAIL\_USERNAME 一致
* MAIL\_FROM\_NAME=Lartisan —— 用来作为邮件的发送者名称






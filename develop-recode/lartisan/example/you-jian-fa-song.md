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




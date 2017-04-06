# 认证授权代码解析

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

生成的配置文件config/auth.php

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




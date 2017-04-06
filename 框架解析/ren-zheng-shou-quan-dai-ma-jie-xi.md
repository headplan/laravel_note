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
```




# 微信登录

#### 安装 socialiteproviders

https://socialiteproviders.github.io/

为 Laravel Socialite 提供了更多的第三方登录方式 , 基本上需要的 , 都能在这里找到 . 

文档 : http://socialiteproviders.github.io/providers/weixin.html

```
$ composer require socialiteproviders/weixin
```

使用了 laravel 5.5 所以可以省略 ServiceProvider 的设置 . 

设置`EventServiceProvider`

_app/Providers/EventServiceProvider.php_

```php
protected $listen = [
    \SocialiteProviders\Manager\SocialiteWasCalled::class => [
        // add your listeners (aka providers) here
        'SocialiteProviders\Weixin\WeixinExtendSocialite@handle'
    ],
];
```

#### 第三方登录处理流程






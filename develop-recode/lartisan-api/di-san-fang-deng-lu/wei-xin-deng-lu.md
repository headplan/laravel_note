# 微信登录

#### 安装 socialiteproviders

[https://socialiteproviders.github.io/](https://socialiteproviders.github.io/)

为 Laravel Socialite 提供了更多的第三方登录方式 , 基本上需要的 , 都能在这里找到 .

文档 : [http://socialiteproviders.github.io/providers/weixin.html](http://socialiteproviders.github.io/providers/weixin.html)

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

#### 功能调试

**客户端已经获取 access\_token**

因为客户端已经获取了 access\_token , 需要将 access\_token 发给服务器 , 服务器通过 access\_token 获取用户信息 , 如果成功的换取了用户信息则说明 access\_token 正确 , 用户微信登录成功 .

使用tinker调试 . 前面已经记录了如何通过`微信开发者工具`获取一个可用的`access_token`

这里直接在服务端调用 , 注意微信还需要openid

```php
$accessToken = 'ACCESS_TOKEN';
$openID = 'OPEN_ID';
$driver = Socialite::driver('weixin');
$driver->setOpenId($openID);
$oauthUser = $driver->userFromToken($accessToken);
```

##### 客户端只获取授权码

获取到授权码后就提交给服务器 , 服务器完成换取`access_token`及换取用户信息的流程 .

先添加修改一些配置文件 :

_config/services.php_

```php
'weixin' => [
    'client_id' => env('WEIXIN_KEY'),
    'client_secret' => env('WEIXIN_SECRET'),
    'redirect' => env('WEIXIN_REDIRECT_URI'),  
],
```

```
# socialite weixin
WEIXIN_KEY=wx****
WEIXIN_SECRET=d4****
```

在使用tinker测试 : 

```php
$code = 'CODE';
$driver = Socialite::driver('weixin');
$response = $driver->getAccessTokenResponse($code);
$driver->setOpenId($response['openid']);
$oauthUser = $driver->userFromToken($response['access_token']);
```

#### 第三方登录处理流程




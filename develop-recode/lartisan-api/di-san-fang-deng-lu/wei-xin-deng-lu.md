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

最终服务器获取了用户在微信的用户信息 , 这一点很重要 , 无论使用上面哪种方式 , 都一定要保证用户数据是服务器自己通过 access\_token 获取的 , 还有这样才能验证客户端提交数据\( code 或 access\_token \)的真实性 . 

现在有两种情况 : 

* 用户第一次使用微信登录 , 根据微信的数据 , 在应用中创建一个用户 , 返回该用户的登录凭证 . 
* 用户已经使用过微信登录 , 则找到数据库中对应的用户 , 返回该用户的登录凭证 . 

这里分辨用户是否已存在 , 就需要一个用户的唯一标识 . 任何一个第三方平台 , 返回的用户信息都会有一个唯一标识 , `socialite`已经为我们封装好了 , 直接使用`$oauthUser->getId()`即可获取 . 

对于微信来说 , 这个唯一标识叫做`openid`, 微信还有一个`unionid`的概念 , 是针对开发者的 , 开发者拥有多个移动应用、网站应用、和公众帐号 , 用该标识区分 . 


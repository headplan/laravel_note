# 微信登录

#### 安装socialiteproviders扩展

socialiteproviders扩展为Laravel Socialite 提供了更多的第三方登录方式 , 基本上你需要的 , 都能在这里找到 .

> [https://socialiteproviders.github.io/](https://socialiteproviders.github.io/)

安装参考 : [http://socialiteproviders.github.io/providers/weixin.html](http://socialiteproviders.github.io/providers/weixin.html)

```
composer require socialiteproviders/weixin
```

使用了 laravel 5.5 所以可以省略 ServiceProvider 的设置 . 然后 ,

设置`EventServiceProvider`app/Providers/EventServiceProvider.php

#### 功能调试

**客户端已经获取 access\_token**

因为客户端已经获取了 access\_token , 需要将 access\_token 发给服务器 , 服务器通过 access\_token 获取用户信息 , 如果成功的换取了用户信息则说明 access\_token 正确 , 用户微信登录成功 .

在tinker中测试 :

```
$accessToken = 'ACCESS_TOKEN';
$openID = 'OPEN_ID';
$driver = Socialite::driver('weixin');
$driver->setOpenId($openID);
$oauthUser = $driver->userFromToken($accessToken);
```

假设我们已经在客户端 , 也就是微信开发者工具中获取了access\_token .

> 只有微信获取用户信息需要同时使用access\_token及openid , 如果你开发的是微博等其他的第三方登录 , 并不需要 $driver-&gt;setOpenId\($openID\); 这一步 .

**客户端只获取授权码 code**

客户端不保存`app_secret`, 获取到授权码后就提交给服务器 , 服务器完成换取`access_token`及换取用户信息的流程 .

这种方式是推荐的安全做法 , 客户端不保存 app\_secret , 获取到授权码后就提交给服务器 , 服务器完成换取 access\_token 及换取用户信息的流程 .

使用这种方式 , 首先需要在服务端配置 app\_id 及 app\_secret :

修改配置文件 , config/services.php

```php
'weixin' => [
    'client_id' => env('WEIXIN_KEY'),
    'client_secret' => env('WEIXIN_SECRET'),
    'redirect' => env('WEIXIN_REDIRECT_URI'),  
],
```

添加到env中 .

```
# socialite weixin
WEIXIN_KEY=wx35***
WEIXIN_SECRET=d****
```

WEIXIN\_KEY 和 WEIXIN\_SECRET 替换为你自己的 appId 和 appsecret

继续使用tinker测试 .

```php
$code = 'CODE';
$driver = Socialite::driver('weixin');
$response = $driver->getAccessTokenResponse($code);
$driver->setOpenId($response['openid']);
$oauthUser = $driver->userFromToken($response['access_token']);
```

这里的code就是前面微信授权时访问的url的连接跳转时带的code . 


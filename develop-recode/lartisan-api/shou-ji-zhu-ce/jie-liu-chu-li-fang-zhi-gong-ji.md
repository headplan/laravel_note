# 节流处理防止攻击

#### 接口安全

基本意思就是对接口的调用频率的限制,限制每个 ip 的调用次数 .

#### 增加调用频率限制

DingoApi 已经为我们提供了调用频率限制的中间件`api.throttle`, 使用起来非常方便 , 修改路由 .

**添加自定义的频率配置**

```php
/**
 * 接口频率限制
 */
'rate_limits' => [
    # 访问频率限制,次数/分钟
    'access' => [
        'expires' => env('RATE_LIMITS_EXPIRES', 1),
        'limit' => env('RATE_LIMITS', 60),
    ],
    # 登录相关,次数/分钟
    'sign' => [
        'expires' => env('SIGN_RATE_LIMITS_EXPIRES', 1),
        'limit' => env('SIGN_RATE_LIMITS', 10),
    ],
],
```

**更新路由 , 调用中间件**

```php
<?php

use Illuminate\Http\Request;

$api = app('Dingo\Api\Routing\Router');

$api->version('v1', [
    'namespace' => 'App\Http\Controllers\Api\v1'
], function($api) {
    $api->group([
        'middleware' => 'api.throttle',
        'limit' => config('api.rate_limits.sign.limit'),
        'expires' => config('api.rate_limits.sign.expires'),
    ], function() {
        # 短信验证码
        $api->post('verificationCodes', 'VerificationCodesController@store')
            ->name('api.verificationCodes.store');
        # 用户注册
        $api->post('users', 'UsersController@store')
            ->name('api.users.store');
    });
});
```




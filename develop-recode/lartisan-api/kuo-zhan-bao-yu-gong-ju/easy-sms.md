# Easy-sms

easy-sms是一个短信发送组件 , 利用这个组件 , 可以快速的实现短信发送功能 . 

```
composer require "overtrue/easy-sms"
```

* 支持目前市面多家服务商
* 一套写法兼容所有平台
* 简单配置即可灵活增减服务商
* 内置多种服务商轮询策略、支持自定义轮询策略
* 统一的返回值格式，便于日志与监控
* 自动轮询选择可用的服务商

添加配置文件config/easysms.php

```php
<?php
return [
    // HTTP 请求的超时时间（秒）
    'timeout' => 5.0,

    // 默认发送配置
    'default' => [
        // 网关调用策略，默认：顺序调用
        'strategy' => \Overtrue\EasySms\Strategies\OrderStrategy::class,

        // 默认可用的发送网关
        'gateways' => [
            'yunpian',
        ],
    ],
    // 可用的网关配置
    'gateways' => [
        'errorlog' => [
            'file' => '/tmp/easy-sms.log',
        ],
        'yunpian' => [
            'api_key' => env('YUNPIAN_API_KEY'),
        ],
    ],
];
```




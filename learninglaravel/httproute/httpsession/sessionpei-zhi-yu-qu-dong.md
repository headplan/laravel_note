# Session配置与驱动

配置文件

```php
<?php

return [
    'driver' => env('SESSION_DRIVER', 'file'),
    'lifetime' => 120, # session的生存时间
    'expire_on_close' => false, # 设置关闭浏览器即过期
    'encrypt' => false, # 加密session数据后再存储
    'files' => storage_path('framework/sessions'), # 文件驱动时,session文件的存储位置
    'connection' => null, # 数据库,redis驱动时,这里的设置对应database配置中的配置
    'table' => 'sessions', # 数据库驱动时,存储session的表
    'store' => null, # acp或memcached驱动时,指定缓存存储区.当然,要与程序已经配置的缓存存储区对应
    'lottery' => [2, 100], # 回收机制的阈值
    'cookie' => 'laravel_session', # sessionID的头信息Key
    'path' => '/', # 配置会话可用路径,一般设置为根路径
    'domain' => env('SESSION_DOMAIN', null), # 用于标识应用程序中的会话的cookie的可用域
    'secure' => env('SESSION_SECURE_COOKIE', false), # 配置cookie仅支持https加密链接
    'http_only' => true, # 配置为true,将阻止javascript访问cookie的值,只能通过http协议访问
];
```

**driver - 驱动程序**

laravel默认支持 - "file", "cookie", "database", "apc", "memcached", "redis", "array"


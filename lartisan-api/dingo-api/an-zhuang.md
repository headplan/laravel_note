# 安装与配置

要安装这个包 , 你需要 :

* Laravel 5.1+ or Lumen 5.1+
* PHP 5.5.9+

新版本的需要PHP ^7.0

编辑composer.json文件

```
"require": {
    "dingo/api": "1.0.*@dev"
}

# 新版本
"require": {
    "dingo/api": "2.0.0-alpha1"
}
```

命令行可以使用 :

```
composer require dingo/api:1.0.x@dev
```

> 1.0的版本都属于开发版 , 还没有 **stable** 版本 , 可能需要设置你的 minimum-stability 为 dev .

#### Laravel

打开config/app.php文件 , 注册必要的 service provider  :

```php
'providers' => [ 
    Dingo\Api\Provider\LaravelServiceProvider::class
]
```

然后运行命令 , 会在config中生成api.php配置文件 :

```php
php artisan vendor:publish --provider="Dingo\Api\Provider\LaravelServiceProvider"
```

> **Lumen**
>
> 打开 bootstrap/app.php , 注册必要的 service providers
>
> ```
> $app->register(Dingo\Api\Provider\LumenServiceProvider::class);
> ```

#### Facades

这个包提供了两个 facades . 可以随意添加任何一个

```
# 这是一个用于api调度的facade,它也为这个包的其他方法提供辅助方法
Dingo\Api\Facade\API
# 这是一个用于API路由的facade,可以用作获取当前路由,请求,检查当前路由名称等
Dingo\Api\Facade\Route
```

### 配置文件

> 其实大部分已经配置好了 , 也可以使用.env去配置 , Lumen可以在bootstrap/app.php文件中配置 , 也可以使用AppServiceProvider中的boot方法 . 当然 , Laravel还是使用命令生成api.php配置文件为好 .

```php
<?php

return [

    'standardsTree' => env('API_STANDARDS_TREE', 'x'),
    'subtype' => env('API_SUBTYPE', ''),
    'version' => env('API_VERSION', 'v1'),
    'prefix' => env('API_PREFIX', null),
    'domain' => env('API_DOMAIN', null),
    'name' => env('API_NAME', null),
    'conditionalRequest' => env('API_CONDITIONAL_REQUEST', true),
    'strict' => env('API_STRICT', false),
    'debug' => env('API_DEBUG', false),
    'errorFormat' => [
        'message' => ':message',
        'errors' => ':errors',
        'code' => ':code',
        'status_code' => ':status_code',
        'debug' => ':debug',
    ],
    'middleware' => [

    ],
    'auth' => [

    ],
    'throttling' => [

    ],
    'transformer' => env('API_TRANSFORMER', Dingo\Api\Transformer\Adapter\Fractal::class),
    'defaultFormat' => env('API_DEFAULT_FORMAT', 'json'),
    'formats' => [
        'json' => Dingo\Api\Http\Response\Format\Json::class,
    ],

];
```

**standardsTree\(标准树\)** : 有三个不同的标准 : x , prs , vnd . 使用的标准树需要取决于你开发的项目 :

* 未注册的树\(x\) - 主要表示本地和私有环境
* 私有树\(prs\) - 主要表示没有商业发布的项目
* 供应商树\(vnd\) - 主要表示公开发布的项目

子类型使用私有和供应商树在**技术上**意味着在 IANA 上注册 , 但是并不强制要求 . 如果不确定该如何选择 , x标准或者说未注册树都是安全的 .

**subtype\(子类型\)** : 一般是你应用或者项目的简称 , 全小写 . \(例如:myapp\)

**version\(版本\)** : API的默认版本 , 用于某些状况下没有指定版本号的时候 . 它也用作生成API文档的默认版本 .

**prefix\(前缀\)和domain\(子域名\)**

大部分的 API 不是有一个子域名就是有一个前缀 . 一个前缀或者子域名是必要的 , **但是只能有一个 . **应避免将版本号作为前缀或者子域名 , 版本的变更应该由 Accept 头控制 .

可以在.env文件中配置 :

```
API_PREFIX=api
#或者
API_DOMAIN=api.myapp.com
```

**name\(名称\)** : 只用在生成文档的时候 , 当生成文档的时候用作默认的名字 , 避免必须去手动定义 . 例如 :

```
API_NAME=My API
# 或者
API_NAME="My API"
```

**conditionalRequest\(条件请求\)** : 默认开启 , 它将利用客户端的缓存机制在可能的情况下缓存 API 请求 . 

**strict\(严格模式\)** : 严格模式需要客户端发送 Accept 头 , 代替配置文件中配置的默认版本 . 这意味着你将不能通过浏览器访问你的 API . 

如果开启严格模式 , 发送非法的Acceept会抛出一个未处理的异常 : 

```
Symfony\Component\HttpKernel\Exception\BadRequestHttpException
```

需要自己处理这个异常 . 

**debug\(调试模式\)** : 当调试模式开启时 , 一般的错误都会被包捕获 , 结果中会包含了一个debug键 , 值为详细的错误追踪记录 . 










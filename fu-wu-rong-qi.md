# 服务容器

在Laravel框架中 , 服务容器是由Illuminate\Container\Container类实现的 , 它实现了服务容器的核心功能 , 而Illuminate\Foundation\Application类继承了这个类 , 主要实现了服务容器的初始化配置和扩展功能 , 对于Laravel框架 , 当接收到一个请求时 , 就会为了处理这个请求首先生成一个服务容器 , 用于容纳处理请求需要的服务 . 

```
# laravel\public\index.php
<?php
// 注册自动加载文件
require __DIR__.'/../bootstrap/autoload.php';
// 服务容器生成
$app = require_once __DIR__.'/../bootstrap/app.php';
```

```
# laravel\bootstrap\app.php
<?php
// 创建服务容器
$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);
// HTTP,Console,Exceptions(异常)类绑定
$app->singleton(
    Illuminate\Contracts\Http\Kernel::class,
    App\Http\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Console\Kernel::class,
    App\Console\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    App\Exceptions\Handler::class
);

return $app;
```




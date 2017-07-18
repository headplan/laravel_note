# 服务容器

**服务容器的产生**

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

autoload.php用于自动加载 , app.php文件实现了服务容器的实例化 , 同时绑定了核心处理类 , 并获得一个全局的服务容器实例$app .

**服务绑定**

生成服务容器后 , 首先要做的就是向其中装载服务 , 也就是所谓的容器绑定 . 可以把这种绑定理解为键值对的绑定 , 一个key对应一个服务 , 对于绑定服务的不同 , 需要用到服务容器中不同的绑定函数来实现 , 主要分为回调函数绑定和实例对象绑定 , 字面意思理解他们即可 . 回调函数绑定还分为普通绑定和单例绑定 .

普通绑定的意思是每次生成该服务的实例对象时都会生成一个新的实例对象 , 也就是在程序的生命周期中 , 可以生成多个这种实例对象 . 单例绑定 , 也就是在生成一个实例对象后 , 再次生成返回第一次生成的对象 , 生命周期中只能生成一个这样的实例对象 , 其实就是设计模式中的单例模式 . 

**代码示例查看 , Learning\_Laravel中Service\_Container分支**


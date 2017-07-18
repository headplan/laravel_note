# 服务容器

#### **服务容器的产生**

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

#### **服务绑定**

生成服务容器后 , 首先要做的就是向其中装载服务 , 也就是所谓的容器绑定 . 可以把这种绑定理解为键值对的绑定 , 一个key对应一个服务 , 对于绑定服务的不同 , 需要用到服务容器中不同的绑定函数来实现 , 主要分为回调函数绑定和实例对象绑定 , 字面意思理解他们即可 . 回调函数绑定还分为普通绑定和单例绑定 .

普通绑定的意思是每次生成该服务的实例对象时都会生成一个新的实例对象 , 也就是在程序的生命周期中 , 可以生成多个这种实例对象 . 单例绑定 , 也就是在生成一个实例对象后 , 再次生成返回第一次生成的对象 , 生命周期中只能生成一个这样的实例对象 , 其实就是设计模式中的单例模式 .

**代码示例查看 , Learning\_Laravel中Service\_Container分支\(commit -m"服务绑定"\)**

通过打印看出 , 服务容器通过$bingdings属性和instances属性来记录本实例中绑定的服务的 , 前者记录了回调函数方式绑定的 , 其中以share值区分是否使用单例 , 后者记录实例对象服务绑定 . 键为服务名称 , 值是实例对象 .

还有一种绑定方式 , 是直接绑定具体类名称 , 其本质是自动生成回调函数的方式 .

**代码示例查看 , Learning\_Laravel中Service\_Container分支\(commit -m"服务绑定"\)**

查看打印内容 , 可以看到只有一条信息在$bingdings属性中 , 这里前者为接口 , 后者为该接口的实现 . 所以绑定服务时可以通过类名或接口名作为服务名\(前者是服务名,后者是绑定的服务\) . 这种绑定方式 , Laravel会通过服务容器中的getClosure\(\)函数自动生成创建该类实例对象的回调函数并进行绑定 .  一般情况下 , 也会在第一个参数使用接口 , 即服务名为接口名 , 回调服务为实现的类 . 这种用接口名绑定服务的好处是实现了松耦合的设计 . 

#### 服务解析

前面已经把服务绑定到了服务容器中 , 之后就可以随时获取了 , 即服务解析 . 


# Service\_Container\_Service\_Providers

> 参考 :
>
> [https://laravel.com/docs/5.4/container](https://laravel.com/docs/5.4/container)
>
> [https://laravel.com/docs/5.4/providers](https://laravel.com/docs/5.4/providers)

新建一个控制器TestController

```
php artisan make:controller TestController
```

在App目录下新建Libs/TestLib.php文件 . 文档中有一句话 , 可以更好的理解容器的绑定 .

> 如果一个类没有基于任何接口那么就没有必要将其绑定到容器 . 容器并不需要被告知如何构建对象 , 因为它会使用 PHP 的反射服务自动解析出具体的对象 .

改造TestLib.php文件 , 使其需要初始化 .

```php
<?php

namespace App\Libs;

class TestLib
{
    private $test;

    public function __construct($test)
    {
        $this->test = $test;
    }

    public function getTest()
    {
        return $this->test;
    }
}
```

开始绑定 , 创建自己的服务提供者

```
php artisan make:provider TestServiceProvider
```

看一下artisan命令生成的服务提供者 , 它继承自Illuminate\Support\ServiceProvider类 .

IoC 是将内部设计的类交给系统去控制 , 但是有些类在初始化的时候 , 需要制定特定的参数 , 或者当你需要将实现类绑定到某个接口 , 这时候就必须对这些依赖进行配置 , 系统才能正确解析并引用 .

**register**

而 register 就是这样一个地方 , 你可以在 register 配置类的依赖 , 绑定实现类到接口 , 设置类的别名等等 .

**boot**

而 boot 方法在 register 方法之后调用 , 这就意味着 , 你无须担心在注入某个实例的时候 , 他还没有被绑定或实例化 .

例如 , 建立了 Test 和 TestApi 两个类 , 前者依赖于后者 , 但是在 register 中不确定那个类先被实例化了 , 那么就可以在 boot 中再对后者进行引用 , 因为此时两个类都已经进行正确的配置 .

**providers**

用于延迟加载的 ServiceProvider , 比如希望在引用的时候再让系统去解析那个类 , 可以设置 $defer 变量为 true 来延迟启动 , 节省开销

```
protected $defer = true;
```

设置了延迟启动 , 需要重写 providers 函数 .

```
public function providers() {
    return [Test::class];
}
```

最后别忘记注册这个提供者到config/app.php文件的providers数组 , 这里都是用了php5.5获取类名的方法::class

---

**容器绑定**

在服务提供者中通过`$this->app`变量访问容器 , 看一下容器接口

```
# 普通绑定
bind($abstract, $concrete = null, $shared = false);
bindIf($abstract, $concrete = null, $shared = false);
# 共享绑定(单例)
singleton($abstract, $concrete = null);
# 绑定实例
instance($abstract, $instance);
# 定义上下文绑定
when($concrete);
```




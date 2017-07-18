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

看一下artisan命令生成的服务提供者 , 它继承自Illuminate\Support\ServiceProvider类






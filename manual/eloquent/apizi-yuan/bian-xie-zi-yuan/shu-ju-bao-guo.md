# 数据包裹

默认情况下 , 当资源响应被转换为json串时 , 顶层资源会被包裹在data键中 , 一般的格式如下 :

```js
{
    "data": [
        {
            "id": 1,
            "name": "Eladio Schroeder Sr.",
            "email": "therese28@example.com",
        },
        {
            "id": 2,
            "name": "Liliana Mayert",
            "email": "evandervort@example.com",
        }
    ]
}
```

这里的顶层键 , 就是data键 , 可以在服务提供者中禁用掉 , 或者在一个程序加载时就会调用的服务提供者中禁用即可 , 这里禁用只针对顶层的data , 其他自定义的data键不会被禁用 : 

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Http\Resources\Json\Resource;

class AppServiceProvider extends ServiceProvider
{
    /**
     * 在注册后进行服务的启动。
     *
     * @return void
     */
    public function boot()
    {
        Resource::withoutWrapping();
    }

    /**
     * 在容器中注册绑定。
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
```

#### 包裹嵌套资源

#### 数据包裹和分页




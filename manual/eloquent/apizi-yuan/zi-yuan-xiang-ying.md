# 资源响应

前面已经有很多例子 , 我们知道 , 资源可以直接在路由和控制器中被返回 , 但有时候需要自定义发送给客户端的 HTTP 响应 , Laravel提供了两种方式 : 

* 可以直接在资源上链式调用response方法 , 添加自定义的响应头 . response方法返回的是方法将返回`Illuminate\Http\Response`实例 , 所以可以自定义响应头 . 

```php
use App\User;
use App\Http\Resources\User as UserResource;

Route::get('/user', function () {
    return (new UserResource(User::find(1)))
                ->response()
                ->header('X-Value', 'True');
});
```

* 还可以在资源中定义一个withResponse方法 , 然后在其中定义自定义的响应头 . 

```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\Resource;

class User extends Resource
{
    /**
     * 将资源转换成数组。
     *
     * @param  \Illuminate\Http\Request
     * @return array
     */
    public function toArray($request)
    {
        return [
            'id' => $this->id,
        ];
    }

    /**
     * 自定义资源响应。
     *
     * @param  \Illuminate\Http\Request
     * @param  \Illuminate\Http\Response
     * @return void
     */
    public function withResponse($request, $response)
    {
        $response->header('X-Value', 'True');
    }
}
```




# 概念综述

Laravel中对资源类的使用很方便 , 每一个资源类都定义了一个`toArray`方法 , 在发送响应时它会返回应该被转化成 JSON 的属性数组

这里可以直接使用`$this`来访问模型属性 , 因为资源类将自动代理属性和方法到底层模型以方便访问 . 

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
            'name' => $this->name,
            'email' => $this->email,
            'created_at' => $this->created_at,
            'updated_at' => $this->updated_at,
        ];
    }
}
```

资源已经定义 , 就可以直接在路由或者控制器中直接返回了 : 

```php
use App\User;
use App\Http\Resources\User as UserResource;

Route::get('/user', function () {
    return new UserResource(User::find(1));
});
```




# 编写资源

资源其实就是把给定的模型装换成数组 , 所以生成的每个资源类都包含了一个toArray方法 , 用来将模型属性转换成可以返回给用的API数组 , 前面已经提到过 , 添加了资源转换数组的方法 , 即toArray方法后 , 就可以在路由或者控制器中返回已经定义的资源了 .

#### 关联

如果要在响应中包含关联资源 , 简单的在toArray方法返回的数组中添加即可 . 这里使用collection方法添加即可 :

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        'posts' => Post::collection($this->posts),
        'created_at' => $this->created_at,
        'updated_at' => $this->updated_at,
    ];
}
```

#### 资源集合

资源是将单个模型转换成数组 , 而资源集合是将多个模型的集合转换成数组 . 所有的资源都提供了collection方法来生成一个临时的资源集合 , 所以没有必要为每一个模型都编写一个资源集合类 , 就像前面关联中 , 也使用了collection方法 :

```php
use App\User;
use App\Http\Resources\User as UserResource;

Route::get('/user', function () {
    return UserResource::collection(User::all());
});
```

一般情况下 , 在需要自定义返回的元数据时 , 则需要定义一个资源集合 , 这里在前面的概述中也提到过 :

```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\ResourceCollection;

class UserCollection extends ResourceCollection
{
    /**
     * 将资源集合转换成数组。
     *
     * @param  \Illuminate\Http\Request
     * @return array
     */
    public function toArray($request)
    {
        return [
            'data' => $this->collection,
            'links' => [
                'self' => 'link-value',
            ],
        ];
    }
}
```

资源集合跟单个资源一样 , 也可以在路由或者控制器中直接返回 , 这里不再举例 , 前面已有 .




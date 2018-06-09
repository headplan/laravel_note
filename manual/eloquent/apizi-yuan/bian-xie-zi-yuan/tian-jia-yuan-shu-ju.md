# 添加元数据

添加元数据前面也提到过 , 直接在toArray方法中添加接口 , 而且不用担心添加一些links , meta键的数据时会和分页冲突 , 自定义添加的元数据 , 或其他数据 ,都会自动和默认的links或者meta数据合并 :

```php
public function toArray($request)
{
    return [
        'data' => $this->collection,
        'links' => [
            'self' => 'link-value',
        ],
    ];
}
```

#### 顶层元数据

比如添加一些顶层的数据作为元数据 , 这时候可以使用with方法添加 , 在其中定义一个元数据数组 , 当资源被作为顶层资源渲染时 , 这个数组会包含在其中 :

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
        return parent::toArray($request);
    }

    /**
     * 返回应该和资源一起返回的其他数据数组。
     *
     * @param \Illuminate\Http\Request  $request
     * @return array
     */
    public function with($request)
    {
        return [
            'meta' => [
                'key' => 'value',
            ],
        ];
    }
}
```

#### 构造资源时添加元数据

这个意思就是在路由或控制器中构造资源实例时添加顶层数据 , 其实所有资源都可以用additional方法在new时候添加 : 

```php
return (new UserCollection(User::all()->load('roles')))
                ->additional(['meta' => [
                    'key' => 'value',
                ]]);
```




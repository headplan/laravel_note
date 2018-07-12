# 用户数据

### 获取个人信息

#### Fractal

Fractal是一个转换层\(transformer\) , API 开发中非常方便的一种开发方法 , 可以帮助我们处理响应数据的结构与复杂的嵌套关系 , 最后将数据返回给客户端 . 可以把 Fractal 理解为 Web 开发中视图 , 控制着 API 的最终数据输出 .

> Laravel 5.5 的新功能`eloquent-resources`整体思路跟`Fractal`一致 , 用法也基本相同 .

#### 数据结构

Fractal提供了3中基本机构 :

* DataArraySerializer 结构类似 eloquent-resources 的 Data Wrapping
* ArraySerializer 结构类似 eloquent-resources 的 withoutWrapping
* JsonApiSerializer 结构出自JSON-API , 是一套 json 接口响应规范

常用的结构是 DataArraySerializer 和 ArraySerializer .

DataArraySerializer

```php
# 带着 meta 信息的单条数据
[
    'data' => [
        'foo' => 'bar'
    ],
    'meta' => [
        ...
    ]
];

# 带着 meta 信息的数据集合
[
    'data' => [
        [
            'foo' => 'bar'
        ]
    ],
    'meta' => [
        ...
    ]
];
```

ArraySerializer

```php
# 带着 meta 信息的单条数据
[
    'foo' => 'bar'
    'meta' => [
        ...
    ]
];

# 带着 meta 信息的数据集合
[
    'data' => [
        [
            'foo' => 'bar'
        ]
    ],
    'meta' => [
        ...
    ]
];
```

对于集合来说 , 两者没有差别 , 有 data 和 meta 构成 , 对于 item 来说 , 就是少了一层 data 包裹 . 

* DataArraySerializer 将所有结果的 data 与 meta 区分来 , 结构统一 . 
* 当多个资源嵌套返回的时候 , 例如话题及发布话题的用户 , 多一层 data 会让结构更深一层 . 前端的同学可能会这么显示某个发布话题用户的姓名 , data.topics\[0\].user.data.name , 所以 ArraySerializer 会减少数据嵌套层级 . 

#### 创建转换层

```
mkdir app/Transformers
touch app/Transformers/UserTransformer.php
```

也可以直接在编辑器中创建 . 

**编辑UserTransformer**

```php
<?php

namespace App\Transformers;

use App\Models\User;
use League\Fractal\TransformerAbstract;

class UserTransformer extends TransformerAbstract
{
    public function transform(User $user)
    {
        return [
            'id' => $user->id,
            'name' => $user->name,
            'email' => $user->email,
            'avatar' => $user->avatar,
            'introduction' => $user->introduction,
            'bound_phone' => $user->phone ? true : false,
            'bound_wechat' => ($user->weixin_unionid || $user->weixin_openid) ? true : false,
            'last_actived_at' => $user->last_actived_at->toDateTimeString(),
            'created_at' => $user->created_at->toDateTimeString(),
            'updated_at' => $user->updated_at->toDateTimeString(),
        ];
    }
}
```




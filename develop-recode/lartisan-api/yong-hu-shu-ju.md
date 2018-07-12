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

使用起来很简单 , 只需要给`transformer`方法传入一个模型实例 , 然后返回一个数据即可 , 这个数组就是返回给客户端的响应数据 .

UserTransformer 是可以复用的 , 当前用户信息 , 发布话题用户信息 , 话题回复用户信息都可以用这一个transformer , 这样我们所有的有关 用户 的资源都会返回相同的信息 , 客户端只需要解析一遍即可 . 因为是可复用的 , 特别需要注意一些敏感信息 , 如用户手机 , 微信的union\_id等 , 我们可以使用另外的字段返回 .

* `bound_phone`是否绑定手机
* `bound_wechat`是否绑定微信

或者可以返回phone 但是部分手机数字用 \* 替换 , 总之就是需要保护用户敏感信息 .

#### 获取用户信息

登录获取了 token 之后 , 第一件事就是需要换取用户信息 , 先来实现`获取登录用户信息`接口 .

_routes/api.php_

```php
], function ($api) {
    # 游客可以访问的接口

    # 需要 token 验证的接口
    $api->group(['middleware' => 'api.auth'], function($api) {
        # 当前登录用户信息
        $api->get('user', 'UsersController@me')
            ->name('api.user.show');
    });
});
```

DingoApi 为我们准备好了 api.auth 这个中间件 , 用来区分哪些接口需要验证 token , 哪些不需要 .

编辑控制器 : 

```php
public function me()
{
    return $this->response->item($this->user(), new UserTransformer());
}
```

这里的`$this->user()`等同于`\Auth::guard('api')->user()` , 方便获取到当前登录的用户 , 也就是 token 所对应的用户 . 

`$this->response->item`表示一个单一资源 . 

POSTMAN测试一下接口 . 

没有传入 token 时 , 返回 401 . 

传入token时就正常访问了 . 

#### 响应结构保持统一

```php
{
    "data": {
        "id": 1,
        ...
        ...
        "created_at": "2018-04-30 13:43:13",
        "updated_at": "2018-04-30 13:43:13"
    }
}
```




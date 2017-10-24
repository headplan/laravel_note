# 创建 API Endpoint路由

Endpoint就是路由的另一种术语 , 可以其为端点 , 当我们讨论API时 , 很多人习惯将访问的路由看作Endpoint .

#### 版本号

为了避免和主应用的路由混在一起 , Dingo API使用了自己的路由器 , 正因如此我们首先需要获取API路由器实例来创建Endpoint :

```
$api = app('Dingo\Api\Routing\Router');
```

接下来需要定义版本号 , 从而可以为多版本API创建同样的Endpoint以便后续回滚 :

```
$api->version('v1', function ($api) {

});
```

如果你想要某个组响应多个版本的API可以传递多版本数组 :

```
$api->version(['v1', 'v2'], function ($api) {

});
```

这里的版本号可以看作和框架的标准路由分组一样传递数组属性作为第二个参数 :

```
$api->version('v1', ['middleware' => 'foo'], function ($api) {

});
```

还可以嵌套普通版分组以便后续实现更复杂的自定义Endpoint :

```
$api->version('v1', function ($api) {
    $api->group(['middleware' => 'foo'], function ($api) {
        // Endpoints registered here will have the "foo" middleware applied.
    });
});
```

#### **创建Endpoint**

有了版本号之后就可以开始使用`$api`创建Endpoint了 :

```
$api->version('v1', function ($api) {
    $api->get('users/{id}', 'App\Api\Controllers\UserController@show');
});
```

因为Endpoint以版本号进行分组 , 所以你可以使用同样的URI为同一Endpoint创建不同的响应 :

```
$api->version('v1', function ($api) {
    $api->get('users/{id}', 'App\Api\V1\Controllers\UserController@show');
});

$api->version('v2', function ($api) {
    $api->get('users/{id}', 'App\Api\V2\Controllers\UserController@show');
});
```

还可以使用各自的方法注册资源和控制器 .

> 这里必须指定控制器的完整命名空间 .

**获取路由**

也可以说生成路由 :

```
app('Dingo\Api\Routing\UrlGenerator')->version('v1')->route('users.index');
```

这里除了前面的加载类包 , 还有标注版本号和路由命名 . 路由命名和Laravel中的路由命名方式一样 :

```
$api->get('users/{id}', ['as' => 'users.index', 'uses' => 'Api\V1\UserController@show']);
```

#### **在控制台查看路由**

如果是Laravel5.1+ , 可以直接使用命令查看 :

```
php artisan api:routes
```

---

添加了Facades , 重新整理了之前的路由 : 

```php
DingoRoute::version('v1', function () {
    DingoRoute::group(['namespace'=>'App\Http\Controllers\Api'], function () {
        DingoRoute::get('article', ['as'=>'article.index','uses'=>'ArticlesController@index']);
        DingoRoute::post('article', ['as'=>'article.create','uses'=>'ArticlesController@create']);
        DingoRoute::get('article/{id}',['as'=>'article.show','uses'=>'ArticlesController@show']);
        DingoRoute::put('article/{id}', ['as'=>'article.update','uses'=>'ArticlesController@update']);
        DingoRoute::get('nocon', 'ArticlesController@noCon');
        DingoRoute::get('myres', 'ArticlesController@myRes');
        DingoRoute::get('myerr', 'ArticlesController@myErr');
        DingoRoute::get('myitem/{id}', 'ArticlesController@myItem');
    });
});
```




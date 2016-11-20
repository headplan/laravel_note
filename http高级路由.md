# HTTP高级路由

### 路由命名

```
# route()函数可以获取uri地址
Route::get('/name', ['as' => 'routename', function () {
    return '<b>命名路由:</b>' . route('routename');
}]);
# 其中as和uses是规定的key
Route::get('/Admin/show', [
    'as' => 'show',
    'uses' => 'Admin\HomeController@show'
]);
# name()方法更加便捷
Route::get('Admin/artlist', 'Admin\HomeController@artlist')->name('artlist');
```

### 路由分组

路由分组有几个固定的参数:

* middleware - 中间件
* namespace - 命名空间
* prefix - 路由前缀
* domain - 子域名路由

前面使用文件夹的控制器路由例子:

```
Route::group(['prefix' => 'Admin', 'namespace' => 'Admin'], function () {
    Route::get('show', [
        'as' => 'show',
        'uses' => 'HomeController@show'
    ]);
    Route::get('artlist', 'HomeController@artlist')->name('artlist');
});
```

子域名路由分组可以分配参数:

```
Route::group(['domain' => '{account}.blog.app'], function () {
    Route::get('test/{id}', function ($account, $id = null) {
        if ($account == 'test') {
            return '子域名:' . $account;
        }
    });
});
```


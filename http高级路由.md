# HTTP高级路由

**命名路由**

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




# HTTP基础路由

![](/assets/Snip20161119_1.png)

**使用Postman工具验证HTTP请**

```
# get
Route::get('/get', function () {
    return 'http get';
});
# post
Route::post('/post', function () {
    return 'http post';
});
# put
Route::put('/put', function () {
    return 'http put';
});
# delete
Route::delete('/delete', function () {
    return 'http delete';
});
# patch
Route::patch('/patch', function () {
    return 'http patch';
});
# options
Route::options('/options', function () {
    return 'http options';
});
# match
Route::match(['get', 'post', 'put', 'delete', 'patch', 'options'], '/match' ,function () {
    return 'http match';
});
# any
Route::any('/any', function () {
    return 'http any';
});
# 路由参数
Route::get('/attr/{attr}', function ($id) {
    return 'attr:' . $id;
});
# 路由多参数
Route::get('/posts/{post}/comments/{comment}', function ($postId, $commentId) {
    return 'post:' . $postId . ',comment:' . $commentId;
});
# 路由可选参数
Route::get('/name/{name?}', function ($name = '小明') {
    return 'Name:' . $name;
});
# 正则约束
Route::get('username/{name}', function ($name) {
    return 'Username:' . $name;
})->where('name', '[A-Za-z]+');
Route::get('userid/{id}', function ($id) {
    return 'Userid:' . $id;
})->where('id', '[0-9]+');
Route::get('user/{id}/{name}', function ($id, $name) {
    return 'Username_Userid:' . $name . '_' . $id;
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```

> **注意**:Laravel在其中的post,put,delete,patch等请求中需要提交CsrfToken才可以,为了测试这些请求,可以先把
> 
> app\/Http\/Kernel.php文件中的31行\App\Http\Middleware\VerifyCsrfToken::class,注释掉.
> 
> **注意**:路由参数不能包含 `-` 字符，需要的话可以使用 `_` 替代.\(虽然测试时直接return值是可以的\)
> 
> **注意**:可选参数只能是最后一个参数
> 
> **注意**:如果想要路由参数在全局范围内被给定正则表达式约束,可以使用`pattern`方法.在`RouteServiceProvider`类的`boot`方法中定义约束模式.
> 
> ```
> public function boot(Router $router){
>     $router->pattern('id', '[0-9]+');
>     parent::boot($router);
> }
> ```
> 
> 这样就不用在路由中使用where定义正则约束了.


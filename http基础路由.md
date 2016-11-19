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

```

> 注意:Laravel在其中的post,put,delete,patch等请求中需要提交CsrfToken才可以,为了测试这些请求,可以先把
> 
> app\/Http\/Kernel.php文件中的31行\App\Http\Middleware\VerifyCsrfToken::class,注释掉.


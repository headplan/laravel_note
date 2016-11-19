# HTTP基础路由

![](/assets/Snip20161119_1.png)

**使用Postman工具验证HTTP请求**

```
# get
Route::get('/get', function () {
    echo 'http get';
});
# post
Route::post('/post', function () {
    echo 'http post';
});
# put
Route::put('/put', function () {
    echo 'http put';
});
# delete
Route::delete('/delete', function () {
    echo 'http delete';
});
# patch
Route::patch('/patch', function () {
    echo 'http patch';
});
# options
Route::options('/options', function () {
    echo 'http options';
});
```

> 注意:Laravel在其中的post,put,delete,patch等请求中需要提交CsrfToken才可以,为了测试这些请求,可以先把
> 
> app\/Http\/Kernel.php文件中的31行\App\Http\Middleware\VerifyCsrfToken::class,注释掉


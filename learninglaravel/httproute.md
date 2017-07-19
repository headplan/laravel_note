# HTTP\_Route

所有 Laravel 路由都定义在位于 routes 目录下的路由文件中 , 这些文件通过框架自动加载 . routes/web.php 文件定义了 web 界面的路由 , 这些路由被分配了 web 中间件组 , 从而可以提供 session 和 csrf 防护等功能 . routes/api.php 中的路由是无状态的 , 被分配了 api 中间件组 .

```
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

**查看相关分支的commit**

路由 where 正则约中 , 如果想要全局约束 , 可以在RouteServiceProvider服务提供者的boot中使用pattern方法配置

```
public function boot()
{
    Route::pattern('one', '(.+)');
    parent::boot();
}
```






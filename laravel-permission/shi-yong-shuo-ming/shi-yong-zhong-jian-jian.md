# 使用中间件

包中附带了 `RoleMiddleware` 和 `PermissionMiddleware` 中间件 , 可以直接配置到Kernel.php文件中 : 

```
protected $routeMiddleware = [
    // ...
    'role' => \Spatie\Permission\Middlewares\RoleMiddleware::class,
    'permission' => \Spatie\Permission\Middlewares\PermissionMiddleware::class,
];
```

然后就可以使用中间件保护路由了 : 

```
Route::group(['middleware' => ['role:super-admin']], function () {
    //
});

Route::group(['middleware' => ['permission:publish articles']], function () {
    //
});

Route::group(['middleware' => ['role:super-admin','permission:publish articles']], function () {
    //
});
```

或者将多个角色或者权限用管道符连接 : 

```
Route::group(['middleware' => ['role:super-admin|writer']], function () {
    //
});

Route::group(['middleware' => ['permission:publish articles|edit articles']], function () {
    //
});
```

当然 , 也可以在构造函数中引入中间件 , 保护控制器 : 

```
public function __construct()
{
    $this->middleware(['role:super-admin','permission:publish articles|edit articles']);
}
```

#### 捕获角色和权限失败

如果要覆盖默认的403响应 , 可以使用应用程序的异常处理程序捕获 : 

```
public function render($request, Exception $exception)
{
    if ($exception instanceof \Spatie\Permission\Exceptions\UnauthorizedException) {
        // Code here ...
    }

    return parent::render($request, $exception);
}
```




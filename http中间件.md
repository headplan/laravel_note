# HTTP中间件

中间件其实是在路由上层加了一层过滤或者保护.除了认证过滤之外,还有CORS中间件,日志中间件等.

Laravel默认自带了一些中间件,都在`app/Http/Middleware` 目录中.

**路由中间件**

默认路由中会自动加载路由服务,默认引用web中间件,里面会有session和CSRF保护等.中间件使用列表依然可以使用命令查看:

```
php artisan route:list
```

下面是中间件路由格式:

```
# 中间件路由
Route::get('middle', ['middleware' => ['web', 'auth'], function () {
    session(['key' => 'session_kkk']);
    return '设置session值';
}]);
Route::get('value', function () {
    return '获取session值:' . session('key');
})->middleware(['web', 'auth']);
```

**注册中间件**

编辑kernel.php文件,其中有三个私有属性,分别定义了全局,应用,自定义组三部分路由中间件.

```
'session.login' => \App\Http\Middleware\Sessionlogin::class
```

生成中间件,编辑中间件文件

```
php artisan make:middleware Sessionlogin
```

```
public function handle($request, Closure $next){
    echo '[我是中间件]';
    if (!session('key')) {
        redirect('Admin/login');
    }
    return $next($request);
}
```

编辑route.php路由文件,添加中间件




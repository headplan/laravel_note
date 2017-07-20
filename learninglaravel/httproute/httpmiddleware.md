# HTTP\_Middleware

HTTP 中间件为过滤进入应用的 HTTP 请求提供了一套便利的机制 .

```
php artisan make:middleware TestMiddleware
```

**相关代码查看HTTP\_Middleware分支**

### 路由中间件

#### 中间件之前/之后/终止

一个中间件是请求前还是请求后执行取决于中间件本身 , 下面是命令行创建的中间件 , 在app/Http/Middleware目录中

```php
<?php

namespace App\Http\Middleware;

use Closure;

class TestMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        // 这里是请求之前执行的动作
        return $next($request);
    }
    
    // 请求之后执行
    public function handle($request Closure $next)
    {
        $respone = $next($request);
        return $respone;
    }
}
```




# HTTP\_Middleware

HTTP 中间件为过滤进入应用的 HTTP 请求提供了一套便利的机制 .

```
php artisan make:middleware TestMiddleware
```

文件在app/Http/Middleware目录中

**相关代码查看HTTP\_Middleware分支**

### 路由中间件

#### 中间件之前/之后/终止

一个中间件是请求前还是请求后执行取决于中间件本身 , 定义在中间件的handle方法中 . 前面说的是请求前后的操作 , 有时候中间件可能需要在 HTTP 响应发送到浏览器之后做一些工作 . 比如 , Laravel 内置的“session”中间件会在响应发送到浏览器之后将 Session 数据写到存储器中 , 为了实现这个功能 , 需要定义一个终止中间件并添加 terminate 方法到这个中间件 .

* handle - 请求前
* handle - 请求后
* terminate - 请求终止

#### **Kernel配置和中间件的应用**

中间件的注册 , 围绕着app/Http/Kernel.php文件中的三个数组变量 .

> HTTP 中间件为过滤进入应用的 HTTP 请求提供了一套便利的机制 .

* **$middleware** - 全局中间件 : 数组中的中间件在每一个 HTTP 请求期间被执行
* **$middlewareGroups** - 中间件分组 : 如果想把中间件打包应用到请求中 , 可以在分组中配置 . 
  * 默认自带并应用了web和api两个中间件 , 自动应用在Route的web.php和api.php的路由中
  * 中间件组中可以循环嵌套中间件组 , 就是可以组中组
  * `php artisan route:list`
* **$routeMiddleware** - 路由中间件 : 这里只是一个中间件的别名 , 指向具体路径 , 路由中间件的应用需要定义在路由中 , 或者配置到上面的数组变量中 , 自动加载 . 

**路由中间件的应用**

加载方式::middleware , \[middleware\] , -&gt;middleware

* **直接引入** - ::middleware\(TestMiddleware::class\)
* **定义别名** - 在$routeMiddleware数组中定义别名

支持多个逗号分割或数组传入`->middleware('a', c::class, 'b')`

添加路由组和路由中间件也可以直接写在Route文件中 , 但是不建议这样写 ,这里只是简单引入

* Route::aliasMiddleware\('别名', function\(回调直接写中间件\) , 或者写中间件namespace\)
* Route::middlewareGroup\('组名', \[中间件名或别名组成的数组\]\)

**中间件的其他参数使用冒号:传入handle函数的$next变量之后 , 多个用逗号分割 , 具体查看代码中的VarMiddleware中间件**



**相关代码查看HTTP\_Middleware分支中 , 中间件前置后置相关代码**


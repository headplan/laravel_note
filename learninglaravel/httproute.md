# HTTP\_Route

所有 Laravel 路由都定义在位于 routes 目录下的路由文件中 , 这些文件通过框架自动加载 . routes/web.php 文件定义了 web 界面的路由 , 这些路由被分配了 web 中间件组 , 从而可以提供 session 和 csrf 防护等功能 . routes/api.php 中的路由是无状态的 , 被分配了 api 中间件组 .

**查看相关分支的commit**

**路由整理列表**

* 路由 method 方法
  * get,post,put,patch,delete,options
  * match,any
  * `{{ csrf\_token\(\) }}`
  * `{{ csrf\_field\(\) }}`
  * `{{ method\_field\('PUT'\) }}`
* 路由 scheme 协议 - \[http\] , \[https\]
* 路由 domain 子域名 - \[domain\] , ::domain
* 路由 prefix 前缀 - \[prefix\] , ::prefix , -&gt;prefix
* 路由 where 正则约束 - \[where\] , -&gt;where
* 路由 middleware 中间件 - \[middleware\] , ::middleware , -&gt;middleware
* 路由 namespace 属性 - \[namespace\] , ::namespace
* 路由 uses 属性 - \[uses\] , -&gt;uses
* 路由 as 别名 - \[as\] , ::as , ::name , -&gt;name
* 路由 group 群组属性 - 

路由 where 正则约中 , 如果想要全局约束 , 可以在RouteServiceProvider服务提供者的boot中使用pattern方法配置

```
public function boot()
{
    Route::pattern('one', '(.+)');
    parent::boot();
}
```




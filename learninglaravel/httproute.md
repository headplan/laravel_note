# HTTP\_Route

所有 Laravel 路由都定义在位于 routes 目录下的路由文件中 , 这些文件通过框架自动加载 . routes/web.php 文件定义了 web 界面的路由 , 这些路由被分配了 web 中间件组 , 从而可以提供 session 和 csrf 防护等功能 . routes/api.php 中的路由是无状态的 , 被分配了 api 中间件组 .

**查看相关分支的commit**

### **路由使用**

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
* 路由 group 群组属性 - 群组属性可以分为会被替换的属性和递增型的属性 . 
  * domain为替换型属性
  * prefix , middleware , namespace , as , where这几个属性是递增型属性 , 路由的属性和群组属性会相互结合 . 
  * 群组prefix前缀的优先级低于群组中路由的prefix前缀

> 路由 where 正则约中 , 如果想要全局约束 , 可以在RouteServiceProvider服务提供者的boot中使用pattern方法配置
>
> ```
> public function boot()
> {
>     Route::pattern('one', '(.+)');
>     parent::boot();
> }
> ```

---

### 路由参数与匹配

Laravel 允许在注册定义路由的时候设定路由参数 , 以供控制器或者闭包所用路由参数可以设定在`URI`中，也可以设定在`domain`中 .

* 路由编码匹配 - 自动解码匹配
* 路由参数 - 参数限制
  * 路由参数总是通过花括号进行包裹
  * 路由参数在路由被执行时会被传递到路由的闭包
  * 路由参数不能包含 - 字符 , 需要的话可以使用 \_ 替代
  * 参数顺序按照浏览器输入顺序应用在回调函数中
* 路由可选参数 - URI写{foo?}\(即url中可以不写\) , 可选默认值有两种方式
  * 直接写在函数参数中
  * 写在-&gt;default函数中
* 路由参数正则约束 - 用法前面已经介绍过 , 这里有一点需要注意 , 路由参数不支持斜杠/ . 
  * 如果路由参数中遇到斜杠会报错NotFoundHttpException , 解决办法就是使用正则约束 , 即可正常显示出斜杠
  * `where(['one' => '(.+)']);`

### 路由中间件




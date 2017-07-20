# HTTP\_Controller

控制路由更普遍的方法是使用控制器来组织管理这些行为 , 控制器可以将相关的`HTTP`请求封装到一个类中进行处理 . 通常控制器存放在`app/Http/Controllers`目录中 .

```
php artisan make:controller TestController
```

**查看HTTP\_Controller分支 . **

#### 路由控制器

控制器继承Laravel自带的基类Controller类 , 提供了一些基本的方法 . 基本控制器的创建使用基本的命令行make即可 , 内容参考代码 .

**控制器与命名空间**

`RouteServiceProvider`服务提供者在一个包含命名空间的路由组中加载路由文件 , 因此我们只需要指定类名中`App\Http\Controllers`命名空间之后的部分就可以了 . 

**单操作控制器**

定义一个只处理一个动作的控制器 , 在控制器中定义 \_\_invoke 方法即可 , 单动作控制器注册路由的时候不用指定方法 . 

**控制器中间件**

在控制器的构造函数中使用 middleware 方法可以很轻松的分配中间件给该控制器 , 甚至可以限定该中间件应用到该控制器类的指定方法 . 还可以使用middleware\(\)方式以闭包的方式直接在构造函数中定义中间件 . 




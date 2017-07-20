# HTTP\_Controller

控制路由更普遍的方法是使用控制器来组织管理这些行为 , 控制器可以将相关的`HTTP`请求封装到一个类中进行处理 . 通常控制器存放在`app/Http/Controllers`目录中 .

```
php artisan make:controller TestController
php artisan make:controller PhotoController --resource
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

**前置操作callAction**

在路由控制器初始化之后 , 执行控制器方法之前 , 会先执行callAction方法 , 接收两个参数 , 第一个是方法名 , 第二个是参数 . 方法定义在Laravel代码中的抽象Controller类中 .

**后置\_\_call方法**

这个方法和普通类中的一样 , 如果没有找到类中对应的方法 , 则会调用类的`__call`函数 , 基类中定义了抛出方法不存在的异常 .

#### 资源控制器

* 生成资源控制器 - `php artisan make:controller PhotoController --resource`
  * 绑定资源模型 - `php artisan make:controller PhotoController --resource --model=Photo`
* 注册控制器路由 - `Route::resource('photos', 'PhotoController');`
  * 部分资源路由
    * 仅有 - `['only' => ['index', 'show']]`
    * 除了 - `['except' => ['create', 'store', 'update', 'destroy']]`
  * 重命名路由别名 - `['names' => ['show' => 'photo.build']]`\(注:和部分资源路由在一个数组中\)
  * 重命名路由参数名 - `['parameters' => ['photo' => 'var']]`\(对应关系\)
  * 重命名URL动作名称 - 在AppServiceProvider服务提供者的boot中使用Route::resourceVerbs配置映射数组
* 附加资源控制器方法 - 定义时注意覆盖 , 后面的会覆盖前面的 , 注册方式和普通路由一样 . 

**上面的配置 , 以及选项的修改重写等在route:list命令行中都会完整显示 , 可以辅助查看**

#### 路由参数依赖注入与绑定

服务容器会解析所有控制器 , 可以在控制器的构造函数中类型声明任何依赖 , 这些依赖会被自动解析并注入到控制器实例中 .

**路由参数隐式绑定**

* 使用类型依赖 , 可以在构造函数和方法中使用 , 可以使用Model,或者使用契约类型约束 . 
* 路由参数绑定到控制器方法 , 将路由参数放到其他依赖之后 . 

**路由参数显式绑定**

可以在RouteServiceProvider服务提供者中绑定 , 也可以直接写在路由文件中

* bind\(\)函数绑定参数 - 可以绑定回调函数 , 也可以绑定类方法处理参数 , 可以显示指定 , 或者绑定类在其中定义bind方法
  * `Route::bind('var1', '\App\Http\Controllers\PhotoController@testBind');`
  * `Route::bind('var2','\App\Http\Controllers\PhotoController');`
* model\(\)函数绑定参数 - 为参数绑定数据库模型 , 参数会经过model模型where等固定的方法操作后返回给控制器或路由闭包函数 . 具体如下 :  

```
if ($model = $instance->where($instance->getRouteKeyName(), $value)->first()) {
     return $model;
}
```

* model绑定的第三个参数 , 定义一个回调函数 , 定义上面的判断操作没有信息时返回的内容

```
Route::model('mod','\App\Photo', function ($v) {
    return $v."没有数据";
});
```

> **注:这里的绑定是对所有路由同名的参数进行操作的.**




# Laravel5.1基础内容

#### 安装

```
composer create-project laravel/laravel Learning_Laravel 5.1.33
```

##### 运行方式

使用PHP内置服务器

```
php -S localhost:8888 -t public
```

使用Laravel artisan启动服务\(其实就是运行了上面的命令,但默认端口为8000\)

```
php artisan serve
```

#### 基本工作流程

##### 操作路由

```php
# /app/Http/routes.php
Route::get('/about', function() {
    return 'Hello World';
});

# view('welcome')函数加载试图文件
# 视图存储路径 : /resources/views/welcome.blade.php
# blade是Laravel内置的模版引擎
# 模版分路径书写方式可以使用/或者.
Route::get('/about', function() {
    return view('sites.about');
});
# 然后再在视图文件存储路径创建文件即可.
```

通常路由会链接控制器接受并返回数据.

##### 创建控制器

```php
php artisan make:controller SitesController
# 如果需要创建无预定义方法的控制器,可以传入plain参数
php artisan make:controller TestController --plain
```

命令行创建控制器非常方便,然后编辑路由,再在控制器的index方法中返回一个模版文件

```php
Route::get('/about', 'SitesController@index');
# 然后编辑控制器
return view('sites.about');
```

#### 向视图传递变量

模版中接参数的方式

```
<h2>Hello: <?= $name;?></h2>
<h2>Hello: <?php echo $name;?></h2>
<h2>Hello: {{ $name }}</h2>
# 下面的写法为不转意变量内容的方式,变量中的html会被加载
<h2>Hello: {!! $name !!}</h2>
```

控制器中传递变量的方式

```
public function about()
{
    $name = [];
    $name['first'] = '小';
    $name['last'] = '明';
    $other_name = '小红';
    return view('sites.about',compact('name','other_name'))->with($name);
}
# with()可以传递一个变量,起别名,可以传递一个数组,以键为别名
# view()第二个参数可以传递一个数组,键为别名
# 传递数组,键为别名.传递变量,自定义别名
# 可以使用compact()函数组合变量为数组传递
```






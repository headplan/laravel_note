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

#### Blade模板引擎

创建公共部分layout.blade.php

```
<html>
<head>
    <title>标题</title>
</head>
<body>
<h1>我是公共模版</h1>
    @yield('content')
</body>
</html>
```

之后就可以在其他模版中@extends继承这个layout模版,然后使用@section插入具体内容到@yield中

```
@extends('layout')
@section('content')
<h1>我是一个模版</h1>
<h2>hello: <?= $name;?></h2>
<h2>hello: <?php echo $name;?></h2>
<h2>hello: {{ $name }}</h2>
<h2>hello: {!! $name !!}</h2>
@stop
```

> 这里结尾可以使用@endsection或@stop都可以

在blade模版中if判断和foreach循环的使用

```
@extends('layout')
@section('content')
<h1>Contact Page</h1>
@if(count($name) > 0)
    <ul>
        @foreach($name as $n)
            <li>{{ $n }}</li>
        @endforeach
    </ul>
@elseif(count($name) < 2)
    elseif
@else
    else
@endif
@stop
@section('footer')
    <script>alert('contact page');</script>
@stop
```

#### 环境配置

.env文件存储了比较重要的基本配置,其他的基本配置都在config目录下,比如数据库配置database.php等.

这里着重说一下eng\(\)这个函数,这个函数的第一个参数就是.env配置文件中的key,第二个参数的含义是如果第一个参数的值不存在,则使用第二个参数.

这样多此一举的写法,是为了更好的版本管理和团队开发.

> 其实.env这个文件是放在.gitignore中的

#### 数据库版本控制Migration

在路径中/database/migrations中存放了创建数据库的"配置文件",执行下面的命令,即可在数据库中创建表.

```
php artisan migrate
# 衍生出的还有相应的回滚命令
php artisan migrate:rollback
```

执行命令后所有的表结构都删除了,当然数据也会被删除.这样就可以方便的去修改字段名字了.

下面就是创建自己的migration了,依然使用命令行创建

```
php artisan make:migration create_article_table --create=article(这个参数写表的名字)
```




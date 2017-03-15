# Laravel Forms 使用

### **创建内容**

**注册路由**

```
Route::get('/articles/create','ArticlesController@create');
Route::post('/articles', 'ArticlesController@store');
```

**编辑控制器**

```

```

**创建视图**

这里引入illuminate/html这个包

```
composer require illuminate/html
```

> Laravel 5.4 已经更名为laravelcollective/html

引入之后需要简单配置

```
config/app.php
下的服务提供者中引入刚刚的类
Illuminate\Html\HtmlServiceProvider::class,
在快捷方式中指定门面类
'Form' => Illuminate\Html\FormFacade::class,
之后就可以使用了
```

创建视图

```
@extends('layout')
@section('content')
    <h1>添加新文章</h1>
    {!! Form::open(['url' => '/articles']) !!}
        <div class="form-group">
            {!! Form::label('title', '标题:') !!}
            {!! Form::text('title', '这是一个标题', ['class' => 'form-control']) !!}
        </div>
        <div class="form-group">
            {!! Form::label('content', '内容:') !!}
            {!! Form::textarea('content', '这是内容', ['class' => 'form-control']) !!}
        </div>
        {!! Form::submit('发表内容', ['class' => 'btn btn-primary form-control']) !!}
    {!! Form::close() !!}
@endsection
```




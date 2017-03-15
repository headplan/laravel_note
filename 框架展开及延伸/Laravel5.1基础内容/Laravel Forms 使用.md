# Laravel Forms 使用

### **创建内容**

**注册路由**

```
Route::get('/articles/create','ArticlesController@create');
Route::post('/articles', 'ArticlesController@store');
```

**编辑控制器**

```php
public function index()
{
    $articles = Article::all();
    $articles = Article::oldest()->get();
    # 常用这种方式
    $articles = Article::latest()->get();

    return view('articles.index',compact('articles'));
}

public function show($id)
{
    $article = Article::findOrFail($id);
    if (is_null($article)) {
        abort(404,'错误');
    }

    return view('articles.show', compact('article'));
}

public function create()
{
    return view('articles.create');
}

public function store(Request $request)
{
    $title = $request->get('title');
    # 接收POST数据
    $post = $request->all();
    $post['publish_at'] = Carbon::now();
    # 数据入库 , Create方法默认过滤掉了token
    Article::create($post);
    # 重定向URI
    return redirect('/articles');
}
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

这部分内容结束之后,就是对表单的验证了,避免一些空提交或其他不想要的数据的提交.


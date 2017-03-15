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
php artisan migrate status
```

执行最后一条命令之后,可以看到已经创建了migration文件,但标注的是N,说明还没有创建表结构,编辑这个文件

```
public function up()
{
    Schema::create('aritcles', function (Blueprint $table) {
        $table->increments('id');
        $table->string('title');
        $table->text('content');
        $table->timestamp('publish_at');
        $table->timestamps();
    });
}
```

然后执行命令`php artisan migrate`创建表,现在可以看到表结构了.

如果还需要向这个表中添加新的字段,需要再创建一个migration文件

```
php artisan make:migration add_intro_column_aritcles --table=aritcles(这个参数写明要添加字段的表)
# 编辑migration文件
public function up()
{
    Schema::table('articles', function (Blueprint $table) {
        $table->string('intro');
    });
}

public function down()
{
    Schema::table('articles', function (Blueprint $table) {
        $table->dropColumn('intro');
    });
}
```

执行命令,就可以添加这个字段了.

如果需要修改一个column字段的名字,需要引入一个包,这样就可以使用renameColumn了.

```
composer require doctrine/dbal
```

> TODO:这部分需要再多看些资料,才能用到实际应用中

#### Eloquent应用

**创建模型**

```
php artisan make:model Article
```

框架约定表名articles的模型名为Article.文件路径为app/Article.php.

进入Psysh命令行操作

**插入一条数据**

```
php artisan tinker
>>> $article = new App\Article;
=> App\Article {#679}
>>> $article->title = 'My first title';
=> "My first title"
>>> $article->content = 'lalala';
=> "lalala"
>>> $article->publish_at = Carbon\Carbon::now();
=> Carbon\Carbon {#681
     +"date": "2017-03-14 12:09:39.000000",
     +"timezone_type": 3,
     +"timezone": "UTC",
   }
>>> $article;
=> App\Article {#679
     title: "My first title",
     content: "lalala",
     publish_at: Carbon\Carbon {#681
       +"date": "2017-03-14 12:09:39.000000",
       +"timezone_type": 3,
       +"timezone": "UTC",
     },
   }
>>> $article->save();
=> true
# 插入数据库完成,其中id为主键自增,create_at和update_at为框架自动插入的时间
# 这里用到了一个时间类Carbon\Carbon
```

**刚刚插入那条数据为**

```
>>> $article->toArray();
=> [
     "title" => "My first title",
     "content" => "lalala",
     "publish_at" => Carbon\Carbon {#681
       +"date": "2017-03-14 12:09:39.000000",
       +"timezone_type": 3,
       +"timezone": "UTC",
     },
     "updated_at" => "2017-03-14 12:11:08",
     "created_at" => "2017-03-14 12:11:08",
     "id" => 1,
   ]
```

**查询刚刚插入的那条数据**

```
>>> $data = App\Article::find(1); # 传入id
=> App\Article {#696
     id: 1,
     title: "My first title",
     content: "lalala222",
     publish_at: "2017-03-14 12:09:39",
     created_at: "2017-03-14 12:11:08",
     updated_at: "2017-03-14 12:11:08",
   }
>>> $data->title = 'Update Title'; # 更新这条数据的title
=> "Update Title"
>>> $data->save();
=> true
```

**使用where条件查询**

```
# where()参数直接写字段,条件,内容.使用get();返回Collection,使用first();返回第一条数据对象
>>> $data_where = App\Article::where('content','like','la%')->get();
=> Illuminate\Database\Eloquent\Collection {#707
     all: [
       App\Article {#708
         id: 1,
         title: "Update Title",
         content: "lalala222",
         publish_at: "2017-03-14 12:09:39",
         created_at: "2017-03-14 12:11:08",
         updated_at: "2017-03-14 12:18:40",
       },
     ],
   }
>>> $data_where = App\Article::where('content','like','la%')->first();
=> App\Article {#701
     id: 1,
     title: "Update Title",
     content: "lalala222",
     publish_at: "2017-03-14 12:09:39",
     created_at: "2017-03-14 12:11:08",
     updated_at: "2017-03-14 12:18:40",
   }
```

**设置fillable属性后,插入和更新数据**

使用create\(\)方法,在插入数据之前,需要先设置fillable的可填充字段

```
class Article extends Model
{
    protected $fillable = ['title','content','publish_at'];
}
```

```
>>> $arr = [
... 'title' => 'Second Title',
... 'content' => 'Second Content',
... 'publish_at' => Carbon\Carbon::now()];
=> [
     "title" => "Second Title",
     "content" => "Second Content",
     "publish_at" => Carbon\Carbon {#695
       +"date": "2017-03-14 12:50:43.000000",
       +"timezone_type": 3,
       +"timezone": "UTC",
     },
   ]
>>> $sec_data = App\Article::create($arr);
```

设置了fillable属性后,更新数据也变得简单了

```
>>> $sec_data->update(['title'=>'Change Title']);
=> true
```

#### 简单的展示列表及详情页面

\[ **列表 **\]

**注册路由**

```
Route::get('/articles', 'ArticlesController@index');
```

**创建控制器**

```
php artisan make:controller ArticlesController --plain
```

**编辑控制器**

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;
use App\Article;

class ArticlesController extends Controller
{
    public function index()
    {
        $articles = Article::all();

        return view('articles.index',compact('articles'));
    }
}
```

**创建视图层**

```
# 创建文件articles/index.blade.php
@extends('layout')
@section('content')
    <h1>Articles</h1>
    <hr>
    @foreach($articles as $article)
        <h2>{{ $article->title }}</h2>
        <article>
            {{ $article->content }}
        </article>
    @endforeach
@endsection
```

\[ **详情** \]

**注册路由**

```
Route::get('/articles/{id}','ArticlesController@show');
```

**编辑方法**

```
public function show($id)
{
    $article = Article::find($id);
    # 这里可以使用简单的判断在$id查不到时做什么
    if (is_null($article)) {
        abort(404);
    }
    # 或者直接使用框架的方法,找不到直接Fail掉
    $article = Article::findOrFail($id);

    return view('articles.show', compact('article'));
}
```

**创建视图层**

```
# 创建文件articles/show.blade.php
@extends('layout')
@section('content')
    <h1>{{ $article->title }}</h1>
    <hr>
    <article>
        <div class="body">
            {{ $article->content }}
        </div>
    </article>
@endsection
```

**视图层URL的几种书写方式**






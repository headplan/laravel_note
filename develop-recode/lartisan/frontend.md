# 前端

**目录说明**

* \node\_modules - 前端资源目录
* \public - 生成后的公共文件 , 基本的css , js目录
  * mix-manifest.json - , 通过Laravel-mix打包的文件会生成`app.asjduiik2l1323879dasfydua23.js`
    , 即`js原文件名+hash+.js后缀`, 这是一个对应关系配置文件 . php中可以直接使用mix\(\)函数调用这个路径 , 与之类似的还有asset\(\)函数 , 不适用带后缀的路径 . 
* \resources - 资源模板目录 , 这里见下文单独的整理 . 
* package.json - npm包管理配置文件
* webpack.mix.js - Laravel的webpack配置文件 , 这里区分了前后台的加载文件

---

**\resources - 资源目录说明**

* \assets - 前端模板资源 , 包括js文件和css文件的模板
  * \js
    * \components - vue组件模板
    * app.js - 前台vue入口文件
    * backend - 后台vue入口文件
    * bootstrap.js - Webpack引导文件
  * \sass
    * \app - 前端CSS模板
    * \backend - 后端CSS模板
    * app.scss - 引导加载
    * backend.scss - 引导加载
* lang
* views

---

```
$ git checkout master
$ git checkout -b static-pages
```

合并与删除分支示例 :

```
$ git merge test-branch
$ git branch -d test-branch
```

**路由准备**

```
# routes/web.php
Route::get('/', 'StaticPagesController@home');
Route::get('/help', 'StaticPagesController@help');
Route::get('/about', 'StaticPagesController@about');
```

**生成控制器**

```
$ php artisan make:controller StaticPagesController
```

**添加控制器操作**

```php
public function home()
{
    return view('static_pages/home');
}

public function help()
{
    return view('static_pages/help');
}

public function about()
{
    return view('static_pages/about');
}
```

**添加视图页面**

```
# resources/views
static_pages/home.blade.php
static_pages/help.blade.php
static_pages/about.blade.php
```

**添加通用视图**

```html
# layouts/app.blade.php
<!DOCTYPE html>
<html lang="{{ app()->getLocale() }}">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- CSRF Token -->
    <meta name="csrf-token" content="{{ csrf_token() }}">

    <title>@yield('title', 'Lartisan') - {{ config('app.name', 'Lartisan') }}</title>

    <!-- Styles -->
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
    @stack('styles')
</head>
<body>
    <div id="app">
        @yield('content')
    </div>

    <!-- Scripts -->
    <script src="{{ asset('js/app.js') }}"></script>
    @stack('scripts')
</body>
</html>
```

**提交合并部署**

```
$ git add -A
$ git commit -m "Finish static pages"
$ git checkout master
$ git merge static-pages
$ git push
$ 发布到testing
```




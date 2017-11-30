# 页面分支

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

```HTML
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




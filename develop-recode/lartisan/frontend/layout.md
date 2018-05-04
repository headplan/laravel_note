# 布局

添加布局通用模板和公共部分引入模板文件夹

* common/ - 公共模板
  * error.blade.php - 错误提示
  * message.blade.php - 全局消息
* layouts/ - 布局模板
  * app.blade.php - 布局模板
  * header.blade.php - 头部导航
  * footer.blade.php - 底部页脚

error.blade.php

```
@if(count($errors) > 0)
    @foreach($errors as $error)
        {{ $error }}
    @endforeach
@endif
```

message.blade.php - 布局模板 , 约定关键字message,success,danger

```
@if (Session::has('success'))
    <div class="notification is-success">
        <button class="delete"></button>
        {{ Session::get('message') }}
    </div>
@endif
```

app.blade.php - 布局模板

```
route_class() # 样式控制函数
```

```php
<!DOCTYPE html>
<html lang="{{ app()->getLocale() }}">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- CSRF Token -->
    <meta name="csrf-token" content="{{ csrf_token() }}">

    <title>@yield('title', 'Lartisan') - {{-- setting('site_name', 'Headplan Team') --}}</title>
    <meta name="keyword" content="{{-- @yield('keyword', setting('seo_keyword', 'php,laravel,lartisan,headplan')) --}}" />
    <meta name="description" content="{{-- @yield('description', setting('seo_description', 'Lartisan的开发记录')) --}}" />

    <!-- Styles -->
    <link href="{{ mix('css/app.css') }}" rel="stylesheet">
    @stack('styles')
</head>
<body>
<div id="app" class="{{ route_class() }}-page">
    @include('layouts.header')
    <section>
        <div class="container">
            @include('common.message')
            @yield('content')
        </div>
    </section>
    @include('layouts.footer')
</div>

<!-- Scripts -->
<script src="{{ mix('js/app.js') }}"></script>
@stack('scripts')
</body>
</html>
```

**引入模板**

使用下面的标签定义

```
@yield('content')
@stack('include')
```

**具体模板**

```
// 继承模板
@extends('_layouts.backend.backend')
// 继承部分
@section('content')
@stop

// 插入模板
@push('include')
    @include('_includes.backend.navbar')
    @include('_includes.backend.sidebar')
@endpush
```

提取公共部分之后就可以添加更多的文件了 .

---

布局是需要用到很多URL , 这里记录几个常用的视图中生成URL的方法 .

* route\(\) 生成指定**路由名称**的 URL , 这里需要在路由中使用name\(\)方法指定路由名称 . 
* asset\(\) 生成资源路径的完整 URL . 
* secure\_url \(\) 使用**HTTPS 协议**生成指定**路径**的完整 URL . 
* url\(\) 生成指定**路径**的完整 URL . 

---

**合并frontends分支代码**

```
git checkout master
git merge frontends
```

**删除static-pages分支**

```
git branch -d static-pages
```

# 目录与文件

**目录说明**

* \node\_modules - 前端资源目录
* \public - 生成后的公共文件 , 基本的css , js目录
  * mix-manifest.json - , 通过Laravel-mix打包的文件会生成`app.asjduiik2l1323879dasfydua23.js`
    , 即`js原文件名+hash+.js后缀`, 这是一个对应关系配置文件 . php中可以直接使用mix\(\)函数调用这个路径 , 与之类似的还有asset\(\)函数 , 不适用带后缀的路径 . 
* \resources - 资源模板目录 , 这里见下文单独的整理 . 
* package.json - npm包管理配置文件
* webpack.mix.js - Laravel的webpack配置文件 , 这里区分了前后台的加载文件
* yarn.lock - 使用yarn作为前端的包管理器 , 这是一个自动生成的锁文件

---

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




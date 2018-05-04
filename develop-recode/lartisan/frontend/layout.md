# 布局

添加布局通用模板和公共部分引入模板文件夹

* common/ - 公共模板
  * error.blade.php - 公共错误提示
* layouts/ - 布局模板
  * 

error.blade.php - 暂未布局

```
@if(count($errors) > 0)
    @foreach($errors as $error)
        {{ $error }}
    @endforeach
@endif
```

```
_includes/nav/main.blade.php
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

**\resources - 资源目录说明**

* \assets - 前端模板资源 , 包括js文件和css文件的模板
  * \js
    * \components - vue组件模板
    * app.js - 前台vue入口文件
    * backend - 后台vue入口文件
    * bootstrap.js - Webpack引导文件
  * \sass
    * \app - 前端CSS模板
      * 其他功能样式文件
    * \backend - 后端CSS模板
      * 其他功能样式文件
    * app.scss - 引导加载
    * backend.scss - 引导加载
    * =====
    * \_variables.scss - 定义样式变量 , 例如颜色
    * helper.scss - 函数助手 , 一些公共的函数 , 比如批量生成的margin和padding
    * overides.scss - 覆盖样式 , 比如覆盖框架原有样式
    * style.scss - 自定义样式
* \lang - 语言包 , 目前只安装了中文扩展翻译的自带语言包 , 其余可自定义添加
* \views - 模板文件目录 , 和前面的样式目录一样 , 也分为主题和前后台 . 包含三类目录 : 
  * 布局类型 : 其中分别是前后台或其他的目录 , 不在目录中的为公共模板
    * \\_includes - 引入包含的模板
    * \\_layouts - 页面布局的模板
* * 功能类型
    * \app - 前台应用模板
    * \backend - 后台应用模板
    * \others - 其他的模板 , 静态模板
  * 扩展模板 : 第三方或者vendor组件生成的模板
    * \vendor - 例如翻页模板

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




# 布局

> **视图布局与引入目录规则已在前面的开篇更新**

添加布局通用模板和公共部分引入模板文件夹

* \_includes/
* \_layouts/

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




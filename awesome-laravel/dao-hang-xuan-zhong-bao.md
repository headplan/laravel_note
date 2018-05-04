# 导航选中包

[https://github.com/letrunghieu/active](https://github.com/letrunghieu/active)

```
# 安装
composer require "hieu-le/active:~3.5"
```

```
function active_class($condition, $activeClass = 'active', $inactiveClass = '')
```

active\_class函数 , 满足条件$condition , 返回$activeClass , 也就是样式 , 默认是active , 否则返回inactiveClass

下面是判断条件 : 

if\_route\(\) - 判断当前对应的路由是否是指定的路由；

if\_route\_param\(\) - 判断当前的 url 有无指定的路由参数。

if\_query\(\) - 判断指定的 GET 变量是否符合设置的值；

if\_uri\(\) - 判断当前的 url 是否满足指定的 url；

if\_route\_pattern\(\) - 判断当前的路由是否包含指定的字符；

if\_uri\_pattern\(\) - 判断当前的 url 是否含有指定的字符；

#### 例子

```
<ul class="nav navbar-nav">
    <li class="{{ active_class(if_route('topics.index')) }}"><a href="{{ route('topics.index') }}">话题</a></li>
    <li class="{{ active_class((if_route('categories.show') && if_route_param('category', 1))) }}"><a href="{{ route('categories.show', 1) }}">分享</a></li>
    <li class="{{ active_class((if_route('categories.show') && if_route_param('category', 2))) }}"><a href="{{ route('categories.show', 2) }}">教程</a></li>
    <li class="{{ active_class((if_route('categories.show') && if_route_param('category', 3))) }}"><a href="{{ route('categories.show', 3) }}">问答</a></li>
    <li class="{{ active_class((if_route('categories.show') && if_route_param('category', 4))) }}"><a href="{{ route('categories.show', 4) }}">公告</a></li>
</ul>
```




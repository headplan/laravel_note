# 基本布局

添加helper函数 , 获取当前路由名称 , 并处理为样式class名称 :

```
function route_class()
{
    return strtr(Route::currentRouteName(), '.', '-');
}
```

其中`Route::currentRouteName()`就是获取路由中`name()`的参数 .

**header**

```
两个属性说明,aria与role 
这些都是HTML5针对html tag增加的属性，一般是为不方便的人士提供的功能，比如屏幕阅读器。
role的作用是描述一个非标准的tag的实际作用。比如用div做button，那么设置div 的 role="button"，辅助工具就可以认出这实际上是个button。
aria的意思是Accessible Rich Internet Application，aria-*的作用就是描述这个tag在可视化的情境中的具体信息。
比如aria-label
正常情况下，会在表单里给input组件指定对应的label，当用户tab到输入框时，读屏软件就会读出相应label里的文本。
```

```
# 修改navbar的高
$navbar-height: 5.25rem
# 添加阴影和顶部边框
.navbar {
    border-color: #e7e7e7;
    box-shadow: 0px 1px 11px 2px rgba(42, 42, 42, 0.1);
    border-top: 4px solid #00d1b2;
}
```

**footer**

```
# 解决footer保持在页面底部问题
html {
    position: relative;
    min-height: 100%;
}

body {
    margin-bottom: 60px;
}
# 相对应为到底部
footer {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 60px;
}

# html中清空footer自带的padding : is-paddingless
# 再给content一些mg : m-t-20
```

**创建Home控制器**

```
php artisan make:controller HomeController
```

删除welcome页面 , 创建home/index.blade.php

创建路由 , 控制器方法 . 




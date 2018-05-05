# 基本布局

添加helper函数 , 获取当前路由名称 , 并处理为样式class名称 : 

```
function route_class()
{
    return strtr(Route::currentRouteName(), '.', '-');
}
```

其中`Route::currentRouteName()`就是获取路由中`name()`的参数 . 


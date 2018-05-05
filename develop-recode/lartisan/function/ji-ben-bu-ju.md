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
方法一：footer高度固定+绝对定位
<div id="container">
    <header>HEADER</header>
    <section class="main">MAIN</section >
    <footer>FOOTER</footer>
</div>

# css
*{
    margin: 0;
    padding: 0;
}
html,body{
    height: 100%;
}
#container{
    /*保证footer是相对于container位置绝对定位*/
    position:relative;  
    width:100%;
    min-height:100%; 
    /*设置padding-bottom值大于等于footer的height值，以保证main的内容能够全部显示出来而不被footer遮盖；*/  
    padding-bottom: 100px;  
    box-sizing: border-box;
}
header{
    width: 100%;
    height: 200px;
    background: #999;
}
.main{
    width: 100%;
    height: 200px;
    background: orange;
}
footer{
    width: 100%;
    height:100px;   /* footer的高度一定要是固定值*/ 
    position:absolute;
    bottom:0px;
    left:0px;
    background: #333;
}
```




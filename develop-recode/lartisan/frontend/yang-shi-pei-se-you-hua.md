#### 配色方案

[https://coolors.co/](https://coolors.co/)

```
#ffffff 背景色
#0facf3 蓝色:logo,图标,按钮
#3c4859 深色:标题,title,选中色,底部深色
#7d8a9b 浅色:普通文本,
    #E7E9EB 浅色:边框颜色,割线颜色
#c5ecfd 浅蓝色:鼠标选中文本背景色

# 蓝色同属性彩色
#FFD500 黄色
#F262D3 粉色
#9574EA 紫色
#3DD94A 绿色
#FF7D00 橙色
```

**样式配色**

```
# 修改_variables.scss文件,此文件在bulma文件之前引入
引入初始变量
# 设置自定义初始变量(Initial variables):颜色,字体.
# 设置派生变量(Derived variables):主题颜色,主题反转色,派生字体,派生边框,派生文字色,派生链接色.
# 设置泛型变量(Generic variables):body背景色,body文字大小.

# 创建logo.放在public/img下.

# 图标整理,查看阿里图标下项目.调整完之后,删除原来的fontawesome图标.

# 头部导航(navbar)
顶部添加横向样式.底部添加阴影.
菜单添加图标,写死分类.
高度调整:$navbar-height:4.25rem;
调整字体颜色,选中和划过背景颜色:
$navbar-item-color:$grey;
$navbar-item-hover-background-color:#f5f5f5;
$navbar-item-active-background-color:#fafafa;
覆盖部分调整字体和图标大小,添加选中底部边框.
登录后下拉菜单添加选中.
添加helper:is-radius,显示圆形头像.
添加helper:is-shadow,按钮加入按钮阴影.

# hero
添加一言,右侧暂时留空,添加二维码等.
```

**修改布局**

```
# 首页添加hero模板,引入hitokoto一言,异步刷新,添加跨域中间件.
# 最有分类大小调整:is-one-quarter-desktop
```

**内容确定**

```
首页,话题,专栏
```




# 视图布局

我们已经有了layouts通用模板 , 现在需要提取公共部分 , 让视图部分更容易维护 .

#### 新建目录及文件

```
_includes/nav/main.blade.php
```

**引入layout模板**

```
@include('_includes.nav.main')
```

提取公共部分之后就可以添加更多的文件了 , 这里整理一下视图文件的目录结构 , 视图内容暂时Copy复制Bulma文档中的代码 . 




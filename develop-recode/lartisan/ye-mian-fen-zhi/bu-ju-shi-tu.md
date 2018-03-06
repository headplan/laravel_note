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

---

布局是需要用到很多URL , 这里记录几个常用的视图中生成URL的方法 .

* route\(\) 生成指定**路由名称**的 URL , 这里需要在路由中使用name\(\)方法指定路由名称 . 
* asset\(\) 生成资源路径的完整 URL . 
* secure\_url \(\) 使用**HTTPS 协议**生成指定**路径**的完整 URL . 
* url \(\) 生成指定**路径**的完整 URL . 

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




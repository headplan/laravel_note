# 样式

> 前面在webpack.mix.js中配置了脚本构建文件的路径 : resources/assets/sass/ , sass目录中包含了所有需要构建的样式模板 .

**修改app.scss文件**

引入bulma和font-awesome , 以及自己创建的一些打包的样式 . 引入都是用 :

```
@import "variables";
@import "~font-awesome/scss/font-awesome";
@import "~bulma/bulma";
@import "~buefy/src/scss/buefy";
@import "helper";
```

这里可以自定义创建一些帮助函数 , 例如循环生成不同的margin等 .


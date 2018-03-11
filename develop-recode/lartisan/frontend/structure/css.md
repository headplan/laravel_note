# 样式

> 前面在webpack.mix.js中配置了脚本构建文件的路径 : resources/assets/sass/ , sass目录中包含了所有需要构建的样式模板 .

**添加引导文件bootstrap.scss**

#### **字体引入**

这里使用了google的免费字体 , 后续再调整 , [https://fonts.google.com/](https://fonts.google.com/)

如果访问慢可以使用nginx代理 , 或者使用国内网站替换 , 例如 , [https://www.font.im/](https://www.font.im/)

**使用方式**

```
// Google Fonts
@import url('https://fonts.googleapis.com/css?family=Roboto:300,400,500');
```

**CSS样式**

```
CSS selector {
  font-family: 'Roboto', sans-serif;
}
```

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


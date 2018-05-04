# 样式

> 前面在webpack.mix.js中配置了脚本构建文件的路径 : resources/assets/sass/ , sass目录中包含了所有需要构建的样式模板 .

### **引导文件bootstrap.scss**

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

#### **定义变量**

```
@import "variables";
```

#### **样式框架引入**

```
// Font Awesome
@import "~font-awesome/scss/font-awesome";

// Bulma
@import "~bulma/bulma";

// Buefy
@import "~buefy/src/scss/buefy";
```

#### **公共样式文件**

```
// Custom SASS Pages
@import "overrides"; // 重写样式
@import "helpers"; // 函数助手样式
@import "styles"; // 风格样式
```

---

### 应用样式

这里区分了前后台 , 分开多个文件 , 同时引入bootstrap文件和相关文件夹中的文件 :

```
@import "bootstrap";

@import "backend/overides";
@import "backend/helper";
@import "backend/login";
```




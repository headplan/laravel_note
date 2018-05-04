# SASS语法简介

Sass 是一种可用于编写 CSS 的语言，起初由 Hampton Catlin 进行设计并由 Natalie Weizenbaum 开发。借助 Sass 我们可以少写很多 CSS 代码，并使样式代码的编写更加灵活多变。

#### **导入文件**

```js
@import "~font-awesome/scss/font-awesome"; # 文件夹导入
@import "helper.scss"; # 文件导入
```

#### **设置变量**

所有的变量均以`$`开头 :

```js
$color: #000;
body{
    background-color: $color;
}
```

#### **样式嵌套**

选择器进行相互嵌套 , 有效减少代码量 :

```js
.nav {
  position: relative;
}
.nav div {
  color: red;
}

.nav h1 {
  margin-top: 10px;
}
# 可以直接写成
.nav {
  position: relative;
  div {
    color: red;
  }

  h1 {
    margin-top: 10px;
  }
}
```

#### 引用父选择器

使用`&`对父选择器进行引用 , 添加状态时很好用 :

```js
# 常见的a标签
a {
  color: white;
}

a:hover {
  color: blue;
}
# 可以写成
a {
  color: white;
  &:hover {
    color: blue;
  }
}
```

```js
li {
  &.separator {
    border-top: 1px solid #ccc;
    margin: 5px 0;
  }
  &>a {
    margin: 5px 0;
    padding: 5px 0 5px 15px;
    display: block;

    &:hover {
      background-color: #f5f5f5;
    }
  }
}
```




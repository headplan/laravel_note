# 前端

#### 框架及扩展

**Bulma**

当前版本 : v0.7.1

一个纯CSS的类Bootstrap框架 , 没有使用jQuery完成一些组件 , 可以方便的引入Vue等框架

[https://bulma.io/](https://bulma.io/)

Bulma作者的网站

* [https://htmlreference.io/](https://htmlreference.io/) - html指南
* [https://cssreference.io/](https://cssreference.io/) - css指南
* [https://jgthms.com/web-design-in-4-minutes/](https://jgthms.com/web-design-in-4-minutes/) - 经验分享
* [https://marksheet.io/](https://marksheet.io/) - 经验分享
* [https://jgthms.com/wysiwyg.css/](https://jgthms.com/wysiwyg.css/) - CSS工具 , 只有一个class将html展示成像Markdown一样的样式.
* [https://jgthms.com/minireset.css/](https://jgthms.com/minireset.css/) - CSS工具 , 一个轻量的CSS复位工具 . 

**wysiwyg.css**

使用.

**buefy**

bulma对应的Vue组件

当前版本 : v0.6.5

[https://buefy.github.io/\#/](https://buefy.github.io/#/)

**基本常用样式库**

[http://basscss.com/](http://basscss.com/) - 暂未使用 , 改用SCSS生成m1,p1等边距类

[http://clrs.cc/](http://clrs.cc/) - 暂未使用

**font-awesome**

字体图标

[http://fontawesome.dashgame.com/](http://fontawesome.dashgame.com/)

```
"font-awesome": "^4.7.0"
```

**Google Fonts**

特殊在字体需求可以引入 .

[https://fonts.google.com/](https://fonts.google.com/)

如果访问慢可以使用nginx代理 , 或者使用国内网站替换 , 例如 , [https://www.font.im/](https://www.font.im/)

```
// Google Fonts
@import url('https://fonts.googleapis.com/css?family=Roboto:300,400,500');
```

引入后即可使用

```
CSS selector {
  font-family: 'Roboto', sans-serif;
}
```

**Laravel默认包及扩展**

```
"devDependencies": {
    "axios": "^0.17",
    "bootstrap-sass": "^3.3.7",
    "cross-env": "^5.1",
    "jquery": "^3.2",
    "laravel-mix": "^1.0",
    "lodash": "^4.17.4",
    "vue": "^2.5.7"
}
```

> NPM 是 Node.js（一个基于 Google V8 引擎的 JavaScript 运行环境）的包管理和分发工具

**axios**

axios 一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中

```js
# https://www.kancloud.cn/yunye/axios/234845
window.axios = require('axios');
# 也已经在bootstrap.js中引入
```

**bootstrap**

已确认暂不使用

```
"bootstrap-sass": "^3.3.7",
```

**cross-env**

cross-env能跨平台地设置及使用环境变量 . 在Laravel中的package.json中就能看到

```
"scripts": {
    "dev": "npm run development",
    "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch-poll": "npm run watch -- --watch-poll",
    "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
    "prod": "npm run production",
    "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
},
```

**jQuery**

js库.

**laravel-mix**

为Laravel定制的Webpack为基础的构建工具.

**lodash**

lodash 一个一致性、模块化、高性能的 JavaScript 实用工具库

使用举例

```
_.chunk(['a', 'b', 'c', 'd'], 2);
# => [['a', 'b'], ['c', 'd']]
# https://www.lodashjs.com/
# 使用_是因为已经在bootstray.js中定义引入
window._ = require('lodash');
```

**Vue**

渐进式JavaScript框架.

**Simditor编辑器**

编辑器


# 前端

#### 框架及扩展

**Bulma**

当前版本 : v0.7.1

一个纯CSS的类Bootstrap框架 , 没有使用jQuery完成一些组件 , 可以方便的引入Vue等框架

[https://bulma.io/](https://bulma.io/)

对应的Vue组件

**buefy**

当前版本 : v0.6.5

[https://buefy.github.io/\#/](https://buefy.github.io/#/)

**基本常用样式库**

[http://basscss.com/](http://basscss.com/)

[http://clrs.cc/](http://clrs.cc/)

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

**目录说明**

* \node\_modules - 前端资源目录
* \public - 生成后的公共文件 , 基本的css , js目录
  * mix-manifest.json - , 通过Laravel-mix打包的文件会生成`app.asjduiik2l1323879dasfydua23.js`
    , 即`js原文件名+hash+.js后缀`, 这是一个对应关系配置文件 . php中可以直接使用mix\(\)函数调用这个路径 , 与之类似的还有asset\(\)函数 , 不适用带后缀的路径 . 
* \resources - 资源模板目录 , 这里见下文单独的整理 . 
* package.json - npm包管理配置文件
* webpack.mix.js - Laravel的webpack配置文件 , 这里区分了前后台的加载文件
* yarn.lock - 使用yarn作为前端的包管理器 , 这是一个自动生成的锁文件

---

**\resources - 资源目录说明**

* \assets - 前端模板资源 , 包括js文件和css文件的模板
  * \js
    * \components - vue组件模板
    * app.js - 前台vue入口文件
    * backend - 后台vue入口文件
    * bootstrap.js - Webpack引导文件
  * \sass
    * \app - 前端CSS模板
      * 其他功能样式文件
    * \backend - 后端CSS模板
      * 其他功能样式文件
    * app.scss - 引导加载
    * backend.scss - 引导加载
    * =====
    * \_variables.scss - 定义样式变量 , 例如颜色
    * helper.scss - 函数助手 , 一些公共的函数 , 比如批量生成的margin和padding
    * overides.scss - 覆盖样式 , 比如覆盖框架原有样式
    * style.scss - 自定义样式
* \lang - 语言包 , 目前只安装了中文扩展翻译的自带语言包 , 其余可自定义添加
* \views - 模板文件目录 , 和前面的样式目录一样 , 也分为主题和前后台 . 包含三类目录 : 
  * 布局类型 : 其中分别是前后台或其他的目录 , 不在目录中的为公共模板
    * \\_includes - 引入包含的模板
    * \\_layouts - 页面布局的模板
* * 功能类型
    * \app - 前台应用模板
    * \backend - 后台应用模板
    * \others - 其他的模板 , 静态模板
  * 扩展模板 : 第三方或者vendor组件生成的模板
    * \vendor - 例如翻页模板

---

#### 测试代码

```
$ git checkout master
$ git checkout -b static-pages
```

合并与删除分支示例 :

```
$ git merge test-branch
$ git branch -d test-branch
```

**路由准备**

```
# routes/web.php
Route::get('/', 'StaticPagesController@home');
Route::get('/help', 'StaticPagesController@help');
Route::get('/about', 'StaticPagesController@about');
```

**生成控制器**

```
$ php artisan make:controller StaticPagesController
```

**添加控制器操作**

```php
public function home()
{
    return view('static_pages/home');
}

public function help()
{
    return view('static_pages/help');
}

public function about()
{
    return view('static_pages/about');
}
```

**添加视图页面**

```
# resources/views
static_pages/home.blade.php
static_pages/help.blade.php
static_pages/about.blade.php
```

**添加通用视图**

```html
# layouts/app.blade.php
<!DOCTYPE html>
<html lang="{{ app()->getLocale() }}">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- CSRF Token -->
    <meta name="csrf-token" content="{{ csrf_token() }}">

    <title>@yield('title', 'Lartisan') - {{ config('app.name', 'Lartisan') }}</title>

    <!-- Styles -->
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
    @stack('styles')
</head>
<body>
    <div id="app">
        @yield('content')
    </div>

    <!-- Scripts -->
    <script src="{{ asset('js/app.js') }}"></script>
    @stack('scripts')
</body>
</html>
```

**提交合并部署**

```
$ git add -A
$ git commit -m "Finish static pages"
$ git checkout master
$ git merge static-pages
$ git push
$ 发布到testing
```




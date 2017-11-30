# 前端构建

前面已经创建了简单的路由和基本的页面 . 这里进一步完善前端相关构建 .

**创建分支**

```
$ git checkout master
$ git checkout -b frontends
```

**配置NPM**

看一下package.json文件 , 这是NPM的配置文件 .

> NPM 是 Node.js（一个基于 Google V8 引擎的 JavaScript 运行环境）的包管理和分发工具

```
{
    "private": true,
    "scripts": {
        "dev": "npm run development",
        "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
        "watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
        "watch-poll": "npm run watch -- --watch-poll",
        "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
        "prod": "npm run production",
        "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
    },
    "devDependencies": {
        "axios": "^0.16.2",
        "bootstrap-sass": "^3.3.7",
        "cross-env": "^5.0.1",
        "jquery": "^3.1.1",
        "laravel-mix": "^1.0",
        "lodash": "^4.17.4",
        "vue": "^2.1.10"
    }
}
```

这里我们不使用bootstrap , 改成bulma以及其vue的包buefy , 图标字体使用font-awesome .

```
"buefy": "^0.6.1",
"bulma": "^0.6.1",
"font-awesome": "^4.7.0"
```

**安装包**

```
$ yarn install --no-bin-links
$ yarn install xxx --dev
```

**前端构建流程**

所有前端的文件 , js或css , 都是通过laravel-mix , 也就是改装后的webpack构建而成的 , 这里查看webpack.mix.js配置文件可以看到素有前端文件的入口是app文件 , 最后生成到public目录 :

```
mix.js('resources/assets/js/app.js', 'public/js')
   .sass('resources/assets/sass/app.scss', 'public/css');
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

这里的内容部分下一篇内容更新 , 这里直接编译生成文件 . commit一下 .

```
$ npm run dev
# 开发是可以跟踪监测自动编译
$ npm run watch-poll
# 生成压缩后的前端文件
npm run production
```

> 这构建编译时报错 , 是因为已经删除了bootstrap , 在js文件中依然引入了 ,删除即可 .



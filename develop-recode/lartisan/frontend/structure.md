# 前端构建

前面已经创建了简单的路由和基本的页面 . 这里进一步完善前端相关构建 .

**创建分支**

```
$ git checkout master
$ git checkout -b frontends
```

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
$ yarn add cross-env
```

这里更新了buefy包以及其扩展和依赖 , 需要注意的是font-awesome 5说是要收费 , 这里的图标还是使用之前4.7版本 .

当前版本

```
npm ls bulma
├─┬ buefy@0.6.3
│ └── bulma@0.6.2  deduped
└── bulma@0.6.2
```

> 更新时出现的warning , 解决放在在这`https://medium.com/gulpjs/gulp-util-ca3b1f9f9ac5`
>
> 更新时总是失败 , 原因都是网络问题 , 这里重新安装了所有 , 打开全局的VPN安装成功了 .

**前端构建流程**

所有前端的文件 , js或css , 都是通过laravel-mix , 也就是改装后的webpack构建而成的 , 这里查看webpack.mix.js配置文件可以看到素有前端文件的入口是app文件 , 最后生成到public目录 :

```
mix.js('resources/assets/js/app.js', 'public/js')
   .sass('resources/assets/sass/app.scss', 'public/css');
```

**构建命令**

```
$ npm run dev
# 开发是可以跟踪监测自动编译
$ npm run watch-poll
# 生成压缩后的前端文件
npm run production
```

> 构建编译时报错 , 是因为已经删除了bootstrap框架 , 在js文件中依然引入了 ,删除即可 .




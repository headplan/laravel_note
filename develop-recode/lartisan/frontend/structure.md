# 前端构建

#### 前端构建流程

编辑package.json文件

```
"devDependencies": {
    "axios": "^0.17",
    "cross-env": "^5.1",
    "jquery": "^3.2",
    "font-awesome": "^4.7.0",
    "buefy": "^0.6.5",
    "bulma": "^0.7.1",
    "wysiwyg.css": "^0.0.3",
    "laravel-mix": "^1.0",
    "lodash": "^4.17.4",
    "vue": "^2.5.7"
}

# yarn install --no-bin-links
```

后需添加包

```
$ yarn install xxx --dev --no-bin-links
```

> 更新时出现的warning , 解决放在在这`https://medium.com/gulpjs/gulp-util-ca3b1f9f9ac5`
>
> 更新时总是失败 , 原因都是网络问题 , 这里重新安装了所有 , 打开全局的VPN安装成功了 .

**JS和CSS的引入**

**JS**

resources/assets/js/bootstrap.js - 引导文件 , 通过自定义变量引入了常用的组件 . 去掉bootstrap-sass

```js
try {
    window.$ = window.jQuery = require('jquery');

    # require('bootstrap-sass');
} catch (e) {}
```

resources/assets/js/app.js - 应用脚本文件 , 引入自定义脚本

```js
require('./bootstrap');

window.Vue = require('vue');

import Buefy from 'buefy'
Vue.use(Buefy);

Vue.component('example-component', require('./components/ExampleComponent.vue'));

// const app = new Vue({
//     el: '#app'
// });
```

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




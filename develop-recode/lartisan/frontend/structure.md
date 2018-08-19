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

# yarn install
# 如果install卡在
mozjpeg: ℹ compiling from source
# 需要看装sudo apt-get install libpng16-dev
# 出处https://github.com/imagemin/imagemin-mozjpeg/issues/26
```

后需添加包

```
$ yarn install xxx --dev
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

// 其他的引入
// require('./manage')
```

**CSS**

resources/assets/sass/app.scss - 应用样式引入文件

```js
// Fonts
@import url("https://fonts.googleapis.com/css?family=Raleway:300,400,600");

// Variables 自定义变量
@import "variables";

// Font Awesome
@import "~font-awesome/scss/font-awesome";

// Bulma
@import "~bulma/bulma";

// Buefy - Bulma Vue Modules
@import "~buefy/src/scss/buefy";

// Other SCSS Files
@import "overrides";    // 重写样式
@import "helpers";      // 函数助手样式
@import "styles";       // 风格样式
```

resources/assets/sass/\_helpers.scss - 可用的class

```js
// wysiwyg
@import "~wysiwyg.css/wysiwyg";

// m-t-5 = margin-top: 5px
$spaceamounts: (0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 60, 70, 80, 90, 100); // Adjust this to include the pixel amounts you need.
$sides: (top, bottom, left, right); // Leave this variable alone

@each $space in $spaceamounts {
  @each $side in $sides {
    .m-#{str-slice($side, 0, 1)}-#{$space} {
      margin-#{$side}: #{$space}px !important;
    }

    .p-#{str-slice($side, 0, 1)}-#{$space} {
      padding-#{$side}: #{$space}px !important;
    }
  }
}
```

**构建命令**

```
$ npm run dev
# 开发是可以跟踪监测自动编译
$ npm run watch-poll
# 生成压缩后的前端文件
npm run production
```

**前端引入版本**

```
# webpack.mix.js后面添加.version();
# 模板中使用mix()函数引入
{{ mix('js/app.js') }}
```

> 构建编译时报错 , 是因为已经删除了bootstrap框架 , 在js文件中依然引入了 , 删除即可 .

---

引入bulma扩展包

```
yarn add bulma-extensions
```




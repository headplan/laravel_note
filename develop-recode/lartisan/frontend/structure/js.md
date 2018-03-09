# 脚本

> 前面在webpack.mix.js中配置了脚本构建文件的路径 : resources/assets/js/app.js , js目录中包含了所有需要构建的脚本 .

#### 目录结构描述

bootstrap.js - 引导文件 , 通过自定义变量引入了常用的组件 .

```js
# lodash:一个一致性、模块化、高性能的 JavaScript 实用工具库
# 这样是我们在package.json中配置安装的
# 例如:_.chunk(['a', 'b', 'c', 'd'], 2);
# => [['a', 'b'], ['c', 'd']]
# https://www.lodashjs.com/
window._ = require('lodash');

# 引入jQuery
try {
    window.$ = window.jQuery = require('jquery');
} catch (e) {}

# axios:一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中
window.axios = require('axios');

# 定义了csrf-token
let token = document.head.querySelector('meta[name="csrf-token"]');

...其他注释部分待更新
```

app.js - 应用脚本文件 , 引入自定义脚本

```js
# 引导文件
require('./bootstrap');
# Vue
window.Vue = require('vue');
# Vue组件
import Buefy from 'buefy'
Vue.use(Buefy);
Vue.component('example', require('./components/Example.vue'));

const app = new Vue({
    el: '#app',
    data: {}
});
```

\components - Vue组件文件目录


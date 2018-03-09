# 脚本

前面在webpack.mix.js中配置了脚本构建文件的路径 : resources/assets/js/app.js , js目录中包含了所有需要构建的脚本 . 

#### 目录结构描述

bootstrap.js - 引导文件 , 通过自定义变量引入了常用的组件 . 

```js
# lodash:一个一致性、模块化、高性能的 JavaScript 实用工具库
# 这样是我们在package.json中配置安装的
# 例如:_.chunk(['a', 'b', 'c', 'd'], 2);
# => [['a', 'b'], ['c', 'd']]
# https://www.lodashjs.com/
window._ = require('lodash');
```




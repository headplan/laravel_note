# 使用 laravel-mix 问题汇总 {#articleTitle}

在Mac上发生问题的主要原因是安装时多添加了一个属性 , 导致不断报错 , Mac系统上直接安装即可 : 

```
yarn install
```

> 不用添加`--no-bin-links`属性

#### Windows系统

在Windows系统上安装的流程是 : 

```
npm install --no-bin-links
```

提示`cross-env:not found`错误

```
> @ development Project/Lartisan_example
> cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

sh: cross-env: command not found
```

**原因**

在windows系统上使用`NODE_ENV=production`这样的方式来设置`node`环境时 , 因为windows的系统变量是`%ENV_VAR%`格式 , 为了解决这个跨平台环境变量的问题 , 需要使用`cross-env`组件 . **在Mac和Linux系统不需要安装 , 安装还会报错 , 因为历史路径问题 . **

**解决**

在Mac和Linux系统中直接删除package.json中命令里的cross-env即可 .

如果是Windows系统 , 安装后在命令里写完整的加载路径就可以了 , 或者全局安装 .

在package.json中替换cross-env

```
"development": "node node_modules/cross-env/dist/bin/cross-env.js
```

如果报错

```
no such file or directory , scandir ‘…/node_modules/node-sass/vendor
```

或者

```
sh: 1: node_modules/webpack/bin/webpack.js: Permission denied
```

重建节点即可\(Windows系统\)

```
npm rebuild node-sass --no-bin-links
```

重建了一下 , 然后整个删除了node\_modules文件夹 , 重新yarn install :

```
yarn install
```

运行正常 .


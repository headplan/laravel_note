# 提示cross-env not found的原因及解决办法

新建了项目 , 完成了前端扩展包的安装 , 运行命令npm run dev命令报错 .

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

---

其实问题仅是`cross-env`组件更新后路径问题 , 在本地测试的环境中 , 直接修改使用`"cross-env": "^3.2.3"`版本 , 也可以避免类似错误 .

详解解释查看先关文章

[https://segmentfault.com/a/1190000013203967](https://segmentfault.com/a/1190000013203967)


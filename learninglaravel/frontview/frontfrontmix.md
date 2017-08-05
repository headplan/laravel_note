# Front\_FrontMix

Laravel 默认使用NPM安装管理前端的依赖 , 最新版本默认使用Webpack打包这些前端资源 , Laravel封装为Laravel Mix .

**package.json**

在 Laravel 的根目录中的`Package.json`文件 , 可以找到默认配置安装的前端资源包

```
"devDependencies": {
    "axios": "^0.16.2",
    "bootstrap-sass": "^3.3.7",
    "cross-env": "^5.0.1",
    "jquery": "^3.1.1",
    "laravel-mix": "^1.0",
    "lodash": "^4.17.4",
    "vue": "^2.1.10"
}
```

**webpack.mix.js**

该文件也在根目录 , 也是Laravel Mix的配置 , 用来定义 Webpack 的编译任务 . Mix 支持许多常见的 CSS 与 JavaScript 预处理器 .

```
mix.js('resources/assets/js/app.js', 'public/js')
    .sass('resources/assets/sass/app.scss', 'public/css');
```

### Laravel Mix

相关内容查看\[基本功能\]笔记中的**前端**-&gt;**编译前端资源**

代码演示查看**Front\_FrontMix**分支


# 编译前端资源

#### Laravel Mix

Laravel Mix 提供了简洁流畅的 API , 让你能够为你的 Laravel 应用定义 Webpack 的编译任务 . Mix 支持许多常见的 CSS 与 JavaScript 预处理器 , 通过简单的方法管理资源 .

**安装**

```
# 安装node和npm
node -v
npm -v
# 安装Laravel Mix,其中的package.json就是node的依赖配置,也可以使用yarn安装
npm install
# Win系统或者虚拟机中添加参数
npm install --no-bin-links
```

**应用**

> Mix 基于 Webpack 的配置 , 所以运行定义于 package.json 文件中的 NPM 脚本即可执行 Mix 的编译任务

```
// 运行所有 Mix 任务...
npm run dev

// 运行所有 Mix 任务和压缩资源输出
npm run production

// 监控资源文件修改,一旦资源文件发生变化,Webpack会自动重新编译
npm run watch

// 同上,但会强制让Webpack会自动重新编译
npm run watch-poll
```

#### 使用样式

> 项目根目录的`webpack.mix.js`文件是资源编译的入口 . 可以把它看作是 Webpack 的配置文件 . Mix 任务可以使用链式调用的写法来定义你的资源文件该如何进行编译 .

**Less**

```
# 把 app.less 编译为 public/css/app.css
mix.less('resources/assets/less/app.less', 'public/css');

# 多次调用 less 方法可以编译多个文件
mix.less('resources/assets/less/app.less', 'public/css')
   .less('resources/assets/less/admin.less', 'public/css');

# 自定义编译后的 CSS 文件名,传递完整路径即可
mix.less('resources/assets/less/app.less', 'public/stylesheets/styles.css');

# 重新配置Less-loader的选项,可以使用第三个参数
mix.less('resources/assets/less/app.less', 'public/css', {
    strictMath: true
});
```

**Sass**

```
# 用法同上,第三个参数也是传递配置到Node-Sass插件的
mix.sass('resources/assets/sass/app.sass', 'public/css', {
    precision: 5
});
```

**Stylus**

```
# 用法同上,第三个参数还可以调用其他stylus插件,当然要先安装
mix.stylus('resources/assets/stylus/app.styl', 'public/css', {
    use: [
        require('rupture')()
    ]
});
```

**PostCSS**

```
# 引入postcss,也可以用npm安装其他应用引入
mix.sass('resources/assets/sass/app.scss', 'public/css')
   .options({
        postCss: [
            require('postcss-css-variables')()
        ]
   });
```

**纯CSS**

```
# 单纯的将多个CSS文件合并成一个文件
mix.styles([
    'public/css/vendor/normalize.css',
    'public/css/vendor/videojs.css'
], 'public/css/all.css');
```

**URL处理**

> Laravel Mix 和 Webpack 默认会找到`example.png`,将它复制到你的`public/images`目录下 , 然后在你生成的样式表中重写`url()`. 但这些对css中的绝对路径是无效的 , 会跳过 .

```
# 禁用上面的功能
mix.sass('resources/assets/app/app.scss', 'public/css')
   .options({
      processCssUrls: false
   });
```

**资源地图**

```
# 资源地图默认是关闭的,调用sourceMaps()方法打开
mix.js('resources/assets/js/app.js', 'public/js')
   .sourceMaps();
```

> 它会带来一些编译成本 , 但在使用编译后的资源文件时可以更方便的在浏览器中进行调试

#### 使用脚本

> Mix也提供了一些函数来帮助你使用JavaScript文件 , 像是编译ECMAScript 2015、模块编译、压缩、及简单的串联纯 JavaScript 文件 . 更棒的是 , 这些都不需要自定义的配置

```
mix.js('resources/assets/js/app.js', 'public/js');
```

上面的一行代码支持 :

* ECMAScript 2015 语法
* Modules
* 编译`.vue`文件
* 针对生产环境压缩代码

**Vendor Extraction\(提取依赖\)**

打算频繁更新应用程序的 JavaScript , 应该考虑将所有的依赖库提取到单独文件中 . 这样 , 对应用程序代码的更改不会影响 vendor.js 文件的缓存 .

```
mix.js('resources/assets/js/app.js', 'public/js')
   .extract(['vue'])
```

`extract`方法接受你希望提取到`vendor.js`文件中的所有的依赖库或模块的数组 .

现在Mix将生成三个文件 , 加载时注意顺序

```
<script src="/js/manifest.js"></script> Webpack显示运行时
<script src="/js/vendor.js"></script> 依赖库
<script src="/js/app.js"></script> 应用代码
```

**React**

Mix 可以自动安装 Babel 插件来支持 React . 你只需要替换你的`mix.js()`变成`mix.react()`即可

```
mix.react('resources/assets/js/app.jsx', 'public/js');
```

在背后 , React 会自动下载 , 并且自动下载适当的`babel-preset-react`Babel 插件

**原生JS**

类似使用`mix.styles()`来组合多个样式表一样 , 你也可以使用`scripts()`方法来合并并且压缩多个 JavaScript 文件

```
mix.scripts([
    'public/js/admin.js',
    'public/js/dashboard.js'
], 'public/js/all.js');
```

还可以使用mix.bable\(\)方法代替他 , 目的是为了把所有 ES2015 的代码转换为原生 JavaScript . 文件会经过 Babel 编译 .

**自定义Webpack配置**

Laravel Mix 默认引用了一个预先配置的`webpack.config.js`文件 , 以便尽快启动和运行 . 如果自定义配置 , 有两种方式 :

**合并**

Mix 提供了一个有用的`webpackConfig`方法 , 从而允许你合并任意简短的 Webpack 配置覆盖 , 就是直接用这个方法引入webpack配置

```
mix.webpackConfig({
    resolve: {
        modules: [
            path.resolve(__dirname, 'vendor/laravel/spark/resources/assets/js')
        ]
    }
});
```

**引用自己的配置**

第二个选择是拷贝Mix的`webpack.config.js`到自己的项目根目录

```
cp node_modules/laravel-mix/setup/webpack.config.js ./
```

接下来 , 需要更新`package.json`中的 NPM 脚本 , 以确保它们不再直接引用 Mix 的配置文件 . 只需从命令中移除

`--config="node_modules/laravel-mix/setup/webpack.config.js"`

即可 . 完成之后 , 需要按照需要编辑自己的配置文件

#### 复制文件与目录

`copy`方法可以复制文件与目录至新位置 . 当`node_modules`目录中的特定资源需要复制到`public`文件夹时会很有用 .

```
mix.copy('node_modules/foo/bar.css', 'public/css/bar.css');
```

#### 版本与缓存清楚

许多的开发者会在它们编译后的资源文件中加上时间戳或是唯一的 token , 强迫浏览器加载全新的资源文件以取代提供的旧版本代码副本 . 可以使用 version 方法让 Mix 处理它们 .

`version`方法为你的文件名称加上唯一的哈希值 , 以防止文件被缓存

```
mix.js('resources/assets/js/app.js', 'public/js')
   .version();
```

在模板中引入 , 可以使用辅助函数 :

```
<link rel="stylesheet" href="{{ mix('/css/app.css') }}">
```

在开发中通常是不需要版本化 , 你可能希望仅在运行`npm run production`的时候进行版本化

```
mix.js('resources/assets/js/app.js', 'public/js');

if (mix.config.inProduction) {
    mix.version();
}
```

#### Browsersync自动加载刷新

BrowserSync可以监控你的文件变化 , 并且无需手动刷新就可以把你的变化注入到浏览器中 . 可以通过调用mix.browserSync\(\)方法来启用这个功能支持

```
mix.browserSync('my-domain.dev');

// 或者...

// https://browsersync.io/docs/options
mix.browserSync({
    proxy: 'my-domain.dev'
});
```

接着 , 使用`npm run watch`命令来开启 Webpack 的开发服务器 . 现在 , 当你修改一个脚本或者 PHP 文件 , 看着浏览器立即刷新出来的页面来反馈你的改变 .

#### 通知

在有效的情况下 , Mix 会自动为每个捆绑显示操作系统通知 , 这可以给你一个及时的反馈 : 编译成功还是失败 . 不过 , 某些场景下你可能希望禁止这些通知 , 一个例子就是在生成环境服务器触发Mix . 通知可以通过`disableNotifications`方法被停用 :

```
mix.disableNotifications();
```




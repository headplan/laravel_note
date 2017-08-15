# Basic Config

```
composer create-project laravel/laravel ./ "5.4.*" --prefer-dist
```

#### Composer

修改composer.json文件

```
"name": "headplan/lartisan",
"description": "Lartisan.",
"keywords": ["lartisan", "headplan"],
```

```
composer require 'barryvdh/laravel-debugbar' --dev
composer require 'barryvdh/laravel-ide-helper' --dev
```

> 别忘记在代码中配置服务

#### .env和config/

**.env** - 配置数据库 , 其他暂时不变 .

**config/app.php** - 配置应用名称和时区 , 其他暂时不变 .

```
'name' => 'Lartisan',
'timezone' => 'PRC',
```

**package.json**

保留"bootstrap-sass": "^3.3.7", ~~bulma文档不全~~

**yarn install** - 安装依赖

* yarn install
* ~~yarn install bulma~~
* ~~yarn install buefy~~
* yarn install font-awesome

#### JavaScript

**resources/assets/js/app.js**

删除vue组件应用的例子 . ~~引入Buefy组件并使用 .~~

```
// import Buefy from 'buefy';
// Vue.use(Buefy);
```

**resources/assets/js/bootstrap.js**

~~删除对已经移除package的bootstrap-sass的渲染 .~~

#### CSS

**resources/assets/sass/app.scss**

删除原有引入的Google字体和bootstrap , 添加

```
// Font Awesome
@import "~font-awesome/scss/font-awesome";
```

添加SCSS分层

```
// Custom SASS Pages
# 重写
@import "overrides";
# 助手
@import "helpers";
# 样式
@import "styles";
```

运行npm run dev , 检查是否引入正确 .


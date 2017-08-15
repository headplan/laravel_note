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

#### **package.json**

保留"bootstrap-sass": "^3.3.7", ~~bulma文档不全~~

**yarn install** - 安装依赖

* yarn install
* ~~yarn install bulma~~
* ~~yarn install buefy~~
* yarn add font-awesome

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
# 助手 - 添加helpers
@import "helpers";
# 样式
@import "styles";
```

```
# Helpers

$spaceamounts: (5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 75, 100);
$sides: (top, bottom, left, right);

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

运行npm run dev , 检查是否引入正确 .


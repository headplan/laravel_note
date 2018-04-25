# 创建应用

前面已经配置好了基本的开发环境\(Homestead\) .

**配置Composer加速 : **

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

**创建Laravel应用 : **

```
composer create-project laravel/laravel ./ --prefer-dist "5.5.*"
```

**配置本地Hosts : **

```
192.168.66.66 lartisan.bbs
```

前面已经配置过了yaml文件 , 直接访问即可 .

**配置.env文件 : **

```
APP_ENV=local # 本地的开发环境
APP_DEBUG=true # 设定是否在应用报错时显示调试信息
APP_KEY=your_app_key # 生成应用的密钥用于加密一些较为敏感的数据

# 对数据库的连接方式、数据库名、用户名密码等做相关配置
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=lartisan_bbs
DB_USERNAME=homestead
DB_PASSWORD=secret

# 缓存、会话、队列等驱动的相关配置信息
CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync

# Redis配置
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# 邮件相关配置
MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```

调用`getenv('APP_ENV')`将返回`local`

生成新的key和.env文件

```
php artisan key:generate
```

**引入版本控制**

```
$ git init
$ git add -A
$ git commit -m "Initial commit"
```

**添加代码统一风格文件**

.editorconfig

```
# coding styles between different editors and IDEs
# editorconfig.org

root = true

[*]

# Change these settings to your own preference
indent_style = space
indent_size = 4

# We recommend you to keep these unchanged
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = false

[*.{js,html,blade.php,css,scss}]
indent_style = space
indent_size = 4

[*.md]
trim_trailing_whitespace = false
```

#### 配置信息

配置文件都保存在保存在`config`目录中 .

**配置文件说明**

| 文件名称 | 配置类型 |
| :--- | :--- |
| app.php | 应用相关，如项目名称、时区、语言等 |
| auth.php | 用户授权，如用户登录、密码重置等 |
| broadcasting.php | 事件广播系统相关配置 |
| cache.php | 缓存相关配置 |
| database.php | 数据库相关配置，包括 MySQL、Redis 等 |
| filesystems.php | 文件存储相关配置 |
| mail.php | 邮箱发送相关的配置 |
| queue.php | 队列系统相关配置 |
| services.php | 放置第三方服务配置 |
| session.php | 用户会话相关配置 |
| view.php | 视图存储路径相关配置 |

**配置信息函数**

```
# 使用点语法访问,文件名和选项
$value = config('app.timezone');
# 运行时设置配置值,传递数组
config(['app.timezone' => 'America/Chicago']);
```

**调整配置信息**

使用`env()`函数 , 优先读取`.env`文件中的`APP_NAME`选项 , 也即是Lartisan BBS , 如果没有则用默认的第二个参数 .

```
# 时区
'timezone' => 'Asia/Shanghai',
# 默认语言
'locale' => 'zh-CN',
```

提交git .

**自定义辅助函数**

存放在bootstrap目录中 :

```
$ touch bootstrap/helpers.php
```

并在bootstrap/app.php文件顶部引入 :

```
require_once __DIR__ . '/helpers.php';
```

提交git.

#### 基本布局

布局文件统一存放在`resources/views/layouts`文件夹中 , 主要有 :

* app.blade.php —— 主要布局文件 , 项目的所有页面都将继承于此页面 ; 
* header.blade.php —— 布局的头部区域文件 , 负责顶部导航栏区块 ; 
* footer.blade.php —— 布局的尾部区域文件 , 负责底部导航区块 ; 

**app.blade.php**

```php
<!DOCTYPE html>
# 获取的是config/app.php中的locale选项,也就是zh-CN
<html lang="{{ app()->getLocale() }}">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- CSRF Token -->
    # 为了方便前端的 JavaScript 脚本获取 CSRF 令牌
    <meta name="csrf-token" content="{{ csrf_token() }}">
    # 继承此模板的title以及默认值
    <title>@yield('title', 'LaraBBS') - Laravel 进阶教程</title>

    <!-- Styles -->
    # URL连接到的CSS文件
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
</head>

<body>
    # 辅助函数,将当前访问路由转为CSS类名
    <div id="app" class="{{ route_class() }}-page">
        # 引入头部
        @include('layouts._header')

        <div class="container">
            # 继承模板
            @yield('content')

        </div>
        # 引入页脚
        @include('layouts._footer')
    </div>

    <!-- Scripts -->
    # URL连接到的JS文件
    <script src="{{ asset('js/app.js') }}"></script>
</body>
</html>
```

**route\_class\(\)函数**

```php
/**
 * 将当前访问路由转为CSS类名
 * @return mixed
 */
function route_class()
{
    return str_replace('.', '-', Route::currentRouteName());
}
```

**header.blade.php顶部导航**

```html
<nav class="navbar navbar-default navbar-static-top">
    <div class="container">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapse" data-toggle="collapse" data-target="#app-navbar-collapse">
                <span class="sr-only">Toggle Navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a href="{{ url('/') }}" class="navbar-brand">Lartisan</a>
        </div>

        <div class="collapse navbar-collapse" id="app-navbar-collapse">
            <ul class="nav navbar-nav">

            </ul>

            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">登录</a></li>
                <li><a href="#">注册</a></li>
            </ul>
        </div>
    </div>
</nav>
```

**footer.blade.php底部导航**

```html
<footer class="footer">
    <div class="container">
        <p class="pull-left">
            由 <a href="{{ url('/') }}" target="_blank">Lartisan</a> 设计和编码 <span style="color: #e27575;font-size: 14px;">❤</span>
        </p>

        <p class="pull-right"><a href="mailto:headplan@163.com">联系我们</a></p>
    </div>
</footer>
```

#### 首页展示

生成控制器

```
# -n 排除冲突扩展,例如XDebug
php -n artisan make:controller PagesController
```

创建home\(\)方法展示视图 , 视图存放在`pages/home.blade.php` . 绑定路由 , 删除原来的welcome模板 .

#### 调整样式

```
# 可以更换国内镜像
$ yarn config set registry https://registry.npm.taobao.org
$ yarn install
# 生成CSS和JS文件
$ npm run watch-poll
```

**调整头尾样式**

略.




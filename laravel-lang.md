# Laravel-lang

[https://github.com/overtrue/laravel-lang](https://github.com/overtrue/laravel-lang)

Laravel 5 语言包，包含 52 种语言, 基于[caouecs/Laravel-lang](https://github.com/caouecs/Laravel-lang).

#### 安装

```
composer require "overtrue/laravel-lang:~3.0"
```

#### 替换服务提供者

```
Illuminate\Translation\TranslationServiceProvider::class,
# 替换为
Overtrue\LaravelLang\TranslationServiceProvider::class,
```

#### 配置语言

```
# config/app.php
'locale' => 'zh-CN',
```

#### 使用

使用和正常一样 , 在创建resources/lang/zh-CN/文件夹 , 添加自定义语言项 :

```
<?php

return [
    'user_not_exists'    => '用户不存在',
    'email_has_registed' => '邮箱 :email 已经注册过！',
];
```

使用helper函数调用 :

```
echo trans('demo.user_not_exists'); // 用户不存在
```

相同的目录以及文件内容互为替换 , 例如 :

```
# 只替换掉password重置的文字提示
# resources/lang/zh-CN/passwords.php
<?php

return [
    'reset' => '您的密码已经重置成功了，你可以使用新的密码登录了!',
];
```

#### 复制默认的语言包文件

```
php artisan lang:publish zh-CN,zh-HK,th,tk
```




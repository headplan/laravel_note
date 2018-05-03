# mews captcha

[https://github.com/mewebstudio/captcha](https://github.com/mewebstudio/captcha)

```
# 安装
composer require "mews/captcha:~2.0"
# 生成配置
php artisan vendor:publish --provider='Mews\Captcha\CaptchaServiceProvider'
```

#### 配置文件

```php
<?php

return [

    'characters' => '2346789abcdefghjmnpqrtuxyzABCDEFGHJMNPQRTUXYZ',

    'default'   => [
        'length'    => 5,
        'width'     => 120,
        'height'    => 36,
        'quality'   => 90,
    ],

    'flat'   => [
        'length'    => 6,
        'width'     => 160,
        'height'    => 46,
        'quality'   => 90,
        'lines'     => 6,
        'bgImage'   => false,
        'bgColor'   => '#ecf2f4',
        'fontColors'=> ['#2c3e50', '#c0392b', '#16a085', '#c0392b', '#8e44ad', '#303f9f', '#f57c00', '#795548'],
        'contrast'  => -5,
    ],

    'mini'   => [
        'length'    => 3,
        'width'     => 60,
        'height'    => 32,
    ],

    'inverse'   => [
        'length'    => 5,
        'width'     => 120,
        'height'    => 36,
        'quality'   => 90,
        'sensitive' => true,
        'angle'     => 12,
        'sharpen'   => 10,
        'blur'      => 2,
        'invert'    => true,
        'contrast'  => -5,
    ]

];
```

`characters`选项是用来显示给用户的所有字符串 . 

`default`,`flat`,`mini`,`inverse`分别是定义的四种验证码类型 . 

#### 辅助函数

```
# Return Image
captcha();
Captcha::create();

# Return URL
captcha_src();
Captcha::src();

# Return HTML
captcha_img();
Captcha::img();

# 不同类型
captcha_img('flat');
Captcha::img('inverse');
```




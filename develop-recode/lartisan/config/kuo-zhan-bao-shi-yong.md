# 扩展包使用

#### 后端

**验证码包**

```
# 安装
composer require "mews/captcha:~2.0"
# 生成配置
php artisan vendor:publish --provider='Mews\Captcha\CaptchaServiceProvider'
```

**图片裁剪包**

```
# 安装
composer require intervention/image
# 生成配置
php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravel5"
```

#### dev

**Debugbar工具栏**

```
# 安装
composer require "barryvdh/laravel-debugbar:~3.1" --dev
# 生成配置
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
# 修改控制方式
'enabled' => env('APP_DEBUG', false),
```

**生成器包**

```
# 安装
composer require "summerblue/generator:~0.5" --dev
# 生成测试
php artisan make:scaffold Projects --schema="name:string:index,description:text:nullable,subscriber_count:integer:unsigned:default(0)"
```

#### 前端




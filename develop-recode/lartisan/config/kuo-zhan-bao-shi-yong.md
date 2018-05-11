# 扩展包使用

#### 后端

**Laravel-lang中文语言包**

```
composer require "overtrue/laravel-lang:~3.0"
```

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

**选中样式扩展包**

```
# 安装
composer require "hieu-le/active:~3.5"
```

**HTMLPurifier过滤工具**

```
# 安装
composer require "mews/purifier:~2.0"
# 配置
php artisan vendor:publish --provider="Mews\Purifier\PurifierServiceProvider"
```

**Guzzle HTTP请求库**

```
composer require "guzzlehttp/guzzle:~6.3"
```

**中文拼音转换**

```
composer require "overtrue/pinyin:~3.0"
```

**Redis库**

```
composer require "predis/predis:~1.0"
```

**队列管理**

```
# 安装
composer require "laravel/horizon:~1.0"
# 配置
php artisan vendor:publish --provider="Laravel\Horizon\HorizonServiceProvider"
```

**权限管理**

```
# 安装
composer require "spatie/laravel-permission:~2.7"
# 配置
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="migrations"
```

**用户切换工具 sudo-su**

```
# 安装
composer require "viacreative/sudo-su:~1.1"
# 配置
php artisan vendor:publish --provider="VIACreative\SudoSu\ServiceProvider"
```

**后台模板包**

```
# 安装
composer require "summerblue/administrator:~1.1"
# 配置
php artisan vendor:publish --provider="Frozennode\Administrator\AdministratorServiceProvider"
```

**阿里云OOS存储**

```
composer require jacobcyl/ali-oss-storage:dev-master
# 包含所有Laravel文件存储接口
```

---

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

**Laravel-ide-helper**

```
composer require barryvdh/laravel-ide-helper --dev
```




# Entrust

> [https://github.com/Zizaco/entrust](https://github.com/Zizaco/entrust)

### 安装

```
composer require 'zizaco/entrust'
# 配置文件config/app.php
[providers]
Zizaco\Entrust\EntrustServiceProvider::class,
[aliases]
'Entrust'   => Zizaco\Entrust\EntrustFacade::class,
# 命令生成包配置config/entrust.php
php artisan vendor:publish
# 配置用户表名
'providers' => [
    'users' => [
        'driver' => 'eloquent',
        'model' => Namespace\Of\Your\User\Model\User::class,
        'table' => 'users',
    ],
],
```




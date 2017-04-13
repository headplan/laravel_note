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
## Entrust会利用config/auth.php里面的值,去决定使用那个Model和用户表名.
## 编辑配置entrust.php可以进一步自定义表名和命名空间.
'providers' => [
    'users' => [
        'driver' => 'eloquent',
        'model' => Namespace\Of\Your\User\Model\User::class,
        'table' => 'users',
    ],
],

# 使用中间件,添加到routeMiddleware数组中
'role' => \Zizaco\Entrust\Middleware\EntrustRole::class,
'permission' => \Zizaco\Entrust\Middleware\EntrustPermission::class,
'ability' => \Zizaco\Entrust\Middleware\EntrustAbility::class,

#
```




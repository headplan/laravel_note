# Lartisan社区

### 安装扩展包

```
# 基于Laravel_Basic创建
composer create-project laravel/laravel ./ "5.4.*"
# 包含下面3个包
composer require 'barryvdh/laravel-debugbar' --dev
composer require 'barryvdh/laravel-ide-helper' --dev
composer require 'summerblue/generator' --dev
=====git commit
# 修改composer.json和基本配置
"name": "headplan/lartisan",
"description": "Lartisan.",
"keywords": ["lartisan", "headplan"],
'name' => 'Lartisan',
'timezone' => 'PRC',
# 配置上面三个包(Laravel_Basic已经更新)
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider" // 生成配置
php artisan ide-helper:generate // 生成代码对应文档
php artisan make:scaffold Projects --schema="name:string:index,description:text:nullable,subscriber_count:integer:unsigned,default(0)" // 需要时再使用
# 重新生成key
php artisan key:generate
# 生成认证
php artisan make:auth
=====git commit
# 引入权限管理包
composer require 'Zizaco/Entrust'
配置Entrust包
配置数据库表前缀
=====git commit
# 配置前后台分离
config/auth.php
# 创建后台用户模型,参数-m自动创建迁移文件
php artisan make:model Models/Dashbord -m

```

### 设计表

**使用命令**

```
php artisan make:migration create_users_table --create=lartisan_users
# 参数
--create=users # 创建一个新的表
--table=users  # 使用已经存在的表
--path         # Migration文件创建路径
```

**设计表之后使用命令创建表**

```
php artisan migrate
```

### 路由规划




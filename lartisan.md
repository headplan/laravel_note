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

# 重新生成key
php artisan key:generate
# 生成认证
php artisan make:auth
# 
# 过滤生成的文件
=====git commit
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




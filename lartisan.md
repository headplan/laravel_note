# Lartisan社区

### 安装扩展包

```
composer create-project laravel/laravel ./ "5.4.*"

"barryvdh/laravel-debugbar": "~2.3",
"barryvdh/laravel-ide-helper": "~2.3"
# 过滤生成的文件
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




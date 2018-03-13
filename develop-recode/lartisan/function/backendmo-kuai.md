# Backend模块

前后分离 , 创建新的管理员模型以及路由等 .

**生成Auth**

```
php artisan make:auth
```

**创建管理员Auth : 表名和字段名稍有区别**

新建一个迁移表`php artisan make:migration create_admins_table --create=admins`, 迁移文件在`database/migrations`中 : 

```php
Schema::create('admins', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();
    $table->string('title'); # 比Users多了个字段,记录称谓
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
```

```
php artisan make:migration # Backend模块迁移
php artisan make:model # Backend模块模型
```

其他内容参考Learning\_Laravel中Security\_Auth分支 .


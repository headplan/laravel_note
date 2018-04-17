# 微博CRUD

#### 微博模型

创建迁移文件

```
php artisan make:migration create_statuses_table --create=statuses
```

编辑迁移文件 , 添加字段 : 

```php
# 存储微博内容
$table->text('content');
# 存储微博发布者id,添加索引
$table->integer('user_id')->index();
# 给微博创建时间添加索引
$table->index(['created_at']);
```




# Eloquent

Laravel 的 Eloquent ORM 提供了漂亮、简洁的 ActiveRecord 实现来和数据库进行交互。每个数据库表都有一个对应的「模型」可用来跟数据表进行交互。你可以通过模型查询数据表内的数据，以及将记录添加到数据表中。

#### 定义模型

所有的 Eloquent 模型都继承自`Illuminate\Database\Eloquent\Model`类 .

**创建模型实例**

```
php artisan make:model Flight
php artisan make:model Flight --migration
php artisan make:model Flight -m
```

**测试表**

```php
Schema::create('flights', function (Blueprint $table) {
    $table->increments('id');
    $table->string('slug')->unique();
    $table->string('title');
    $table->text('content');
    $table->timestamp('published_at')->index();
    $table->timestamps();
});
```

**Eloquent 模型约定**

Eloquent模型默认情况下会使用类的「下划线命名法」与「复数形式名称」来作为数据表的名称生成规则 , 例如 :

* Article 数据模型类对应`articles`表；
* User 数据模型类对应`users`表；
* BlogPost 数据模型类对应`blog_posts`表；
* Flight 数据模型类对应`flights`表；

**配置约定**

数据表名称 : `protected $table = 'my_flights';`

主键 : `$primaryKey = 'my_id';`

使用非递增或者非数字的主键 : `public $incrementing = false;`

时间戳 : 关闭自动维护`created_at`和`updated_at`字段 , `public $timestamps = false;`

#### 取回多个模型

取回单个模型或集合

添加和更新模型

删除模型

查询作用域

事件


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

执行迁移

```
$ php artisan migrate
```

创建微博模型文件

```
$ php artisan make:model Models/Status
```

**用户和微博之间的关系**

Eloquent 模型让关联的管理和处理变得更加简单 , 支持 :

* 一对一
* 一对多
* 多对多
* 远层一对多
* 多态关联
* 多态多对多关联

以`一对多`为例子 , 在模型中将 Eloquent 关联定义为函数 :

_app/Models/Status.php_

```php
public function user()
{
    return $this->belongsTo(User::class);
}
```

在用户模型中 , 指明一个用户拥有多条微博 :

_app/Models/User.php_

```php
public function statuses()
{
    return $this->hasMany(Status::class);
}
```

#### 显示微博

为了后面方便测试 , 先生成假数据 :

```
php artisan make:factory StatusFactory
php artisan make:seeder StatusesTableSeeder
```

```php
$factory->define(\App\Models\Status::class, function (Faker $faker) {
    $date_time = $faker->date . ' ' . $faker->time;

    return [
        'content' => $faker->text(),
        'created_at' => $date_time,
        'updated_at' => $date_time,
    ];
});
```

```php
public function run()
{
    //$user_ids = ['1', '2', '3'];
    $users = User::where('id', '<', 4)->get();
    $user_ids = $users->map(function ($user) {
        return $user->id;
    })->toArray();

    $faker = app(Faker\Generator::class);

    $statuses = factory(Status::class)->times(100)->make()->each(function ($status) use ($faker, $user_ids) {
        $status->user_id = $faker->randomElement($user_ids);
    });

    Status::insert($statuses->toArray());
}
```

别忘记最后的 : 

```php
$this->call(StatusesTableSeeder::class);
```




# 测试数据

#### 添加测试数据

**创建模型工厂**

```
php artisan make:factory BlogPostFactory
```

```
return [
    'title' => $faker->sentence(mt_rand(3, 10)),
    'content' => join("\n\n", $faker->paragraphs(mt_rand(3, 6))),
    'published_at' => $faker->dateTimeBetween('-1 month', '+3 days'),
];
```

**创建播种机**

```
php artisan make:seeder PostTableSeeder
```

```
public function run()
{
    # 清空表中数据
    App\Models\BlogPost::truncate();
    factory(App\Models\BlogPost::class, 100)->create();
}
```

```
public function run()
{
    # 新版本中已经自动禁用了对批量创建数据的保护(fillable等)
    // Model::unguard();
    $this->call('PostTableSeeder');
    // Model::reguard();
}
```






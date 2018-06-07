# 插入&更新关联模型

#### save方法

例如 , 要给Post模型插入一个新的Comment , 直接在关联上使用save方法插入Comment即可 :

```php
$comment = new App\Comment(['message' => '一条新的评论']);

$post = App\Post::find(1);

$post->comments()->save($comment);
```

需要注意的是 , 这里没有使用动态属性形式 , 而是直接调用了关联方法 .

save方法会自动在新的Comment模型中添加正确的post\_id值 .

保存多个关联模型可以使用saveMany方法 :

```php
$post = App\Post::find(1);

$post->comments()->saveMany([
    new App\Comment(['message' => '一条新的评论。']),
    new App\Comment(['message' => '另一条评论。']),
]);
```

#### create方法

create方法接收一个属性数组 , 创建模型并插入数据库 .

save和create方法不同的是 , save接收的是一个完整的Eloquent模型实例 , 而create接收的是一个纯PHP数组 :

```php
$post = App\Post::find(1);

$comment = $post->comments()->create([
    'message' => '一条新的评论。',
]);
```

当然 , 这里也可以使用createMany方法 , 用来保存多个关联模型 :

```php
$post = App\Post::find(1);

$post->comments()->createMany([
    [
        'message' => '一条新的评论。',
    ],
    [
        'message' => '另一条新的评论。',
    ],
]);
```

#### 更新belongsTo关联




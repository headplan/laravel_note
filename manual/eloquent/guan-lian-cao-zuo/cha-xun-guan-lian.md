# 查询关联

所有类型的关联都是通过方法定义的 , 可以调用方法来获取关联实例 , 然后在其后使用查询语句构造器进行约束 .

```php
return $test->posts()->first();
# 和直接返回属性一样
return $test->posts;
```

加一些条件约束 : 

```php
$user = App\User::find(1);

$user->posts()->where('active', 1)->get();
```




# 序列化模型&集合

#### 序列化成数组

要将模型还有其加载的关联转换成一个数组 , 则可以使用 toArray 方法 . 这个方法是递归的 , 因此 , 所有属性和关联\(包含关联中的关联\)都会被转换成数组 :

```php
$user = App\User::with('roles')->first();

return $user->toArray();
```

也可以将整个集合转换成数组 : 

```php
$users = App\User::all();

return $users->toArray();
```

#### 序列化成JSON

将模型转换成JSON使用toJson方法 , 这和前面的toArray方法的用法一样 , 也可以强制把一个模型或集合转型成一个字符串 , 会自动调用toArray方法 , 当模型或集合被转型成字符串时 , 模型或集合便会被转换成 JSON 格式 , 所以我们直接在路由或者控制器中返回Eloquent对象时 , 也是json格式的 : 

```php
$user = App\User::find(1);

return $user->toJson();

$user = App\User::find(1);

return (string) $user;

Route::get('users', function () {
    return App\User::all();
});
```




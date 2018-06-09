# 序列化模型&集合

#### 序列化成数组

要将模型还有其加载的关联转换成一个数组 , 则可以使用 toArray 方法 . 这个方法是递归的 , 因此 , 所有属性和关联\(包含关联中的关联\)都会被转换成数组 : 

```php
$user = App\User::with('roles')->first();

return $user->toArray();
```



#### 序列化成JSON




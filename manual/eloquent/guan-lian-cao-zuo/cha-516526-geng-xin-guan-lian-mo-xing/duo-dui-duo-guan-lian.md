# 多对多关联

#### 附加/移除

假设一个用户可以拥有多个角色 , 每个角色都可以被多个用户共享 , 给某个用户附加一个角色是通过向中间表插入一条记录实现的 , 使用attach方法 :

```php
$user = App\User::find(1);

$user->roles()->attach($roleId);
```

使用attach方法时 , 也可以通过传递一个数组参数向中间表写入额外数据 :

```php
$user->roles()->attach($roleId, ['expires' => $expires]);
```

与之对应的方法 , 也就是移除用户的角色 , 使用detach方法 , 当然 , 这时候两个模型的数据依然保存在数据库中 , 只是移除了关联的中间表 :

```php
// 移除用户的一个角色...
$user->roles()->detach($roleId);

// 移除用户的所有角色...
$user->roles()->detach();
```

这里的attach和detach方法都允许传入ID数组 :

```php
$user = App\User::find(1);

$user->roles()->detach([1, 2, 3]);

$user->roles()->attach([
    1 => ['expires' => $expires],
    2 => ['expires' => $expires]
]);
```

#### 同步关联

使用sync方法构造多对多关联 , 此方法可以接收ID数组 , 向中间表插入对应关联数据记录 , 所有没放在数组里的IDs都会从中间表里移除 . 所以 , 这步操作完成后 , 只有在数组里的IDs会保留在中间表中 .

```php
$user->roles()->sync([1, 2, 3]);
```

可以通过 ID 传递其他额外的数据到中间表 :

```php
$user->roles()->sync([1 => ['expires' => true], 2, 3]);
```

如果不想移除现有的 IDs , 可以使用`syncWithoutDetaching`方法 :

```php
$user->roles()->syncWithoutDetaching([1, 2, 3]);
```

#### 切换关联

toggle方法 , 切换给定IDs的附加状态 . 如果给定已经附加 , 就被移除 , 如果移除 , 就被附加 :

```php
$user->roles()->toggle([1, 2, 3]);
```

#### 在中间表上保存额外数据

当处理多对多关联时 , `save`方法还可以使用第二个参数 , 它是一个属性数组 , 包含插入到中间表的额外字段数据 . 

```php
App\User::find(1)->roles()->save($role, ['expires' => $expires]);
```

#### 更新中间表记录

如果需要更新中间表中已存在的记录 , 可以使用 updateExistingPivot 方法 . 此方法接收中间记录的外键和一个属性数组进行更新 : 

```php
$user = App\User::find(1);

$user->roles()->updateExistingPivot($roleId, $attributes);
```




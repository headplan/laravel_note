# 条件语句

这里条件的意思是 , 有时可能想要子句只适用于某个情况为真时才执行查询 . 

例如 , 如果给定的输入值出现在传入请求中时 , 可能要判断它能达成某个where语句 , 可以使用when方法来完成此操作 : 

```php
$role = $request->input('role');

$users = DB::table('users')
    ->when($role, function ($query) use ($role) {
        return $query->where('role_id', $role);
    })-get();
```

现在 , 只有当when方法的第一个参数 , 为true时 , 也就是$role为true时 , 闭包里的where语句才会还行 . 否则就不执行 . 

when方法还接受第三个参数 , 这里的第三个闭包参数是当第一个参数为false时执行的 , 也可以理解为默认值 : 

```php
$sortBy = null;

$users = DB::table('users')
                ->when($sortBy, function ($query) use ($sortBy) {
                    return $query->orderBy($sortBy);
                }, function ($query) {
                    return $query->orderBy('name');
                })
                ->get();
```




# 使用直接权限

可以向任何用户授予权限 :

```
$user->givePermissionTo('edit articles');
# 一次赋予多个权限
$user->givePermissionTo('edit articles', 'delete articles');
# 支持数组形式
$user->givePermissionTo(['edit articles', 'delete articles']);
```

取消用户权限 :

```
$user->revokePermissionTo('edit articles');
```

撤销并添加新的权限 :

```
$user->syncPermissions(['edit articles', 'delete articles']);
```

检查用户是否具有权限 :

```
$user->hasPermissionTo('edit articles');
```

检查用户是否具有权限\(列表\) :

```
$user->hasAnyPermission(['edit articles', 'publish articles', 'unpublish articles']);
```

已保存的权限会注册在`Illuminate\Auth\Access\Gate`类中 , for 默认的guard . 所以也可以使用Laravel的默认函数检查权限 :

```
$user->can('edit articles');
```




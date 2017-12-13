# 通过角色使用权限

可以将角色分配给任何用户 :

```
$user->assignRole('writer');
# 一次分配多个角色
$user->assignRole('writer', 'admin');
# 支持数组形式
$user->assignRole(['writer', 'admin']);
```

可以从用户中删除角色 :

```
$user->removeRole('writer');
```

同步角色 , 即删除现有分配 , 绑定新的角色 : 

```
$user->syncRoles(['writer', 'admin']);
```

确定用户是否具有某种角色 : 

```
$user->hasRole('writer');
```

确定用户是否具有给定的角色列表 : 

```
$user->hasAnyRole(Role::all());
```

确定用户是否具有给定的所有角色列表 : 

```
$user->hasAllRoles(Role::all());
```

assignRole、hasRole、hasAnyRole、hasAllRoles 和 removeRole 函数可以接受一个字符串 , 一个`\Spatie\Permission\Models\Role`对象 或者 一个`\Illuminate\Support\Collection`对象 . 

---

可以向角色授予权限 : 

```
$role->givePermissionTo('edit articles');
```

也可以确定某个角色是否具有某种权限 : 

```
$role->hasPermissionTo('edit articles');
```

当然也可以从角色中吊销权限 : 

```
$role->revokePermissionTo('edit articles');
```

givePermissionTo 和 revokePermissionTo 函数可以接受字符串或`Spatie\Permission\Models\Permission`对象 . 

---

权限是从角色中自动继承的 . 此外 , 还可以将个人权限分配给用户 . 例如 : 

```
$role = Role::findByName('writer');
$role->givePermissionTo('edit articles');
# 自动继承角色的权限
$user->assignRole('writer');
# 用户个人权限
$user->givePermissionTo('delete articles');
```

这里call`$user->hasDirectPermission('delete articles')`会返回true , $user-&gt;hasDirectPermission\('edit articles'\)则会返回false . 这里的目的是为了在应用程序中给角色和用户设置权限时 , 去限制用户角色的继承权限 , 即只允许更改用户的直接权限 , 则此方法很有用 . 

---

列出权限 : 

```
// 直接权限
$user->getDirectPermissions() // Or $user->permissions;

// 用户从角色中继承的权限
$user->getPermissionsViaRoles();

// All permissions which apply on the user (inherited and direct)
$user->getAllPermissions();
```




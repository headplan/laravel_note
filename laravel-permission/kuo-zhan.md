# 扩展与缓存

#### 扩展

如果需要扩展或替换现有的角色或权限模型 , 配置下面的内容即可 : 

Role模型实现`Spatie\Permission\Contracts\Role`contract契约

Permission模型实现`Spatie\Permission\Contracts\Permission`contract契约

复制生成配置文件 : 

```
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="config"
```

然后更新其中的 `models.role` 和 `models.permission` 的值 . 

#### 缓存

缓存角色和权限数据以加快性能 . 当使用下面的方法来操作角色和权限时 , 将自动重置缓存 : 

```
$user->assignRole('writer');
$user->removeRole('writer');
$user->syncRoles(params);
$role->givePermissionTo('edit articles');
$role->revokePermissionTo('edit articles');
$role->syncPermissions(params);
```

但是 , 如果是直接在数据库中操作权限/角色数据而不是调用提供的方法 , 则除非手动重置缓存 , 否则不会看到应用程序中反映的更改 . 

**手动重置缓存**

```
php artisan cache:forget spatie.permission.cache
```

#### 缓存标识

这里要注意的是 , 如果正在使用缓存服务 , 例如redis或者memcached . 并且在服务器上还运行着其他站点 , 可能会遇到缓存冲突 . 需要在/config/cache.php中配置缓存前缀prefix , 为了避免其他应用程序意外使用/更改你的缓存数据 . 






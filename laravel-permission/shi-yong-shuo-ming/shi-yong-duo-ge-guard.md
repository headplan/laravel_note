# 使用多个Guard

默认会使用auth配合中的default配置的guard . 无需额外的配置 . 但是 , 当使用多个Guard时 , 他们会充当权限和角色的命名空间一样 . 意味着每个Guard都有自己的一组权限和角色 , 可以分配给他们的用户模型 . 

#### 多Guard的角色与权限使用

默认情况下使用`config('auth.defaults.guard')`配置的默认guard守卫 . 指定guard必须在指定的模型上指定guard\_name属性 , 可以直接在模型中直接定义 , 也可以 : 

```
// 为管理员用户创建管理员角色
$role = Role::create(['guard_name' => 'admin', 'name' => 'superadmin']);

// 为管理员的管理员用户定义 "发布文章" 权限
$permission = Permission::create(['guard_name' => 'admin', 'name' => 'publish articles']);

// 为web用户定义"发布文章" 权限
$permission = Permission::create(['guard_name' => 'web', 'name' => 'publish articles']);
```

检查用户是否具有特定权限 : 

```
$user->hasPermissionTo('publish articles', 'admin');
```

---

#### 分配权限和角色为不同的Guard

使用方式和前面的一样 , 只是需要指定Guard , 即与guard\_name匹配 , 否则将引发 GuardDoesNotMatch 异常 . 

---

#### 多Guard使用Blade模板命令

前面已经提到过 , 只需传入第二个参数为指定guard即可 : 

```
@role('super-admin', 'admin')
    I am a super-admin!
@else
    I am not a super-admin...
@endrole
```




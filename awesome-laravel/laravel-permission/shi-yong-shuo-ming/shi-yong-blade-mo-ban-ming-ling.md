# 使用Blade模板命令

包提供了验证当前登录的用户是否具有给定的所有角色列表等命令 . 

> 可以使用第二个参数传入指定的guard .

#### Blade and Roles

测试指定角色 : 

```
@role('writer')
    I am a writer!
@else
    I am not a writer...
@endrole
```

功能相同的命令 : 

```
@hasrole('writer')
    I am a writer!
@else
    I am not a writer...
@endhasrole
```

测试列表中的任何角色 : 

```
@hasanyrole($collectionOfRoles)
    I have one or more of these roles!
@else
    I have none of these roles...
@endhasanyrole
// or
@hasanyrole('writer|admin')
    I am either a writer or an admin or both!
@else
    I have none of these roles...
@endhasanyrole
```

测试所有角色 : 

```
@hasallroles($collectionOfRoles)
    I have all of these roles!
@else
    I do not have all of these roles...
@endhasallroles
// or
@hasallroles('writer|admin')
    I am both a writer and an admin!
@else
    I do not have all of these roles...
@endhasallroles
```

---

#### Blade and Permissions

包中没有对特定权限的blade模板命令 , 而是使用Laravel自带的@can命令来检查用户是否具有某种权限 : 

```
@can('edit articles')
  //
@endcan
```

或者

```
@if(auth()->user()->can('edit articles') && $some_other_condition)
  //
@endif
```




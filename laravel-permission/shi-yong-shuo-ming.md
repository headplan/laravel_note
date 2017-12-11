# 使用说明

首先 , 添加`Spatie\Permission\Traits\HasRoles Trait`在用户模型 :

```php
use Illuminate\Foundation\Auth\User as Authenticatable;
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;

    // ...
}
```

注意 : 如果要将HasRoles trait使用在其他用户模型中 , 需要添加一个属性 :

```php
use Illuminate\Database\Eloquent\Model;
use Spatie\Permission\Traits\HasRoles;

class Page extends Model
{
   use HasRoles;

   protected $guard_name = 'admin'; // or whatever guard you want to use

   // ...
}
```

否则可能会报错 .

程序包允许用户与权限和角色相关联 . 每个角色都与多个权限相关联 . 角色和权限都是正常的Eloquent模型 , 需要手动创建 :

```php
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

$role = Role::create(['name' => 'writer']);
$permission = Permission::create(['name' => 'edit articles']);
```

注意 , 使用其他的guard , 需要给模型定义guard\_name属性 .

HasRoles trait会将Eloquent关系模型添加到模型中 , 可以直接使用这些方法 :

```php
// 获取直接分配给用户的所有权限列表
$permissions = $user->permissions;

// 获取用户通过角色继承的所有权限
$permissions = $user->getAllPermissions();

// 获取所有已定义角色的集合
$roles = $user->getRoleNames(); // Returns a collection
```

HasRoles trait还添加了一个角色范围到模型 , 可以将查询范围限定为特定的角色或权限 : 

```php
// 获取具有writer角色的用户
$users = User::role('writer')->get();
```




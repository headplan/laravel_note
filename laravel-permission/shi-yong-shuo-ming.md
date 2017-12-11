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




# 数据播种

关于数据库播种的两个说明 : 

最好在播种前刷新 `spatie.permission.cache` 以避免缓存冲突错误 . 可以使用artisan命令或者直接写在播种机类里 . 

下面是一个示例播种机 , 它清除缓存 , 创建权限 , 然后为角色分配权限 : 

```
use Illuminate\Database\Seeder;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

class RolesAndPermissionsSeeder extends Seeder
{
    public function run()
	{
    	// Reset cached roles and permissions
        app()['cache']->forget('spatie.permission.cache');

        // create permissions
        Permission::create(['name' => 'edit articles']);
        Permission::create(['name' => 'delete articles']);
        Permission::create(['name' => 'publish articles']);
        Permission::create(['name' => 'unpublish articles']);

        // create roles and assign existing permissions
        $role = Role::create(['name' => 'writer']);
        $role->givePermissionTo('edit articles');
        $role->givePermissionTo('delete articles');

        $role = Role::create(['name' => 'admin']);
        $role->givePermissionTo('publish articles');
        $role->givePermissionTo('unpublish articles');
    }
}
```




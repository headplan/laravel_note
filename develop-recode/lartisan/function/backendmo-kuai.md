# Backend模块

前后分离 , 创建新的管理员模型以及路由等 .

**生成Auth**

```
php artisan make:auth
```

**创建管理员Auth : 表名和字段名稍有区别**

新建一个迁移表`php artisan make:migration create_admins_table --create=admins`, 迁移文件在`database/migrations`中 :

```php
Schema::create('admins', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();
    $table->string('title'); # 比Users多了个字段,记录称谓
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
```

**然后创建一个Model**

新建Models文件夹 , 复制User.php的内容`php artisan make:model Admin`

```
<?php
# 命名空间
namespace App;
# 这里用到了消息通知,下面有use Notifiable,关于消息通知查看下面的内容
use Illuminate\Notifications\Notifiable;
# 这个是User的Base Model,它也是继承Model的,也叫User.只是起了别名Authenticatable,其继承了3个接口,然后分3个trait完成了接口的功能
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * The attributes that are mass assignable.
     * 表示可以被复制的字段,Eloquent中有解释
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     * 表示可以隐藏的字段
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];
}
```

* 接下来配置新的Guard和Provider , 具体查看实例中的config/auth.php配置
* 创建admin路由和控制器以及视图 . 
* commit一次

其他内容参考Learning\_Laravel中Security\_Auth分支 .


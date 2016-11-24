# ORM模型

**创建模型**

```
php artisan make:model Dbs
```

自动在app目录下创建模型.可以将其移动到其他目录,但要记住修改命名空间.

```
namespace App\Http\Model;
```

模型中有几个常用的基本定义

```
class Dbs extends Model
{
    protected $table = 'user'; # 定义表名
    protected $primaryKey = 'userid'; # 定义主键名
    public $timestamps = false; # 定义排除时间记录字段
}
```


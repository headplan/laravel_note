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

在控制器中使用,需要先引入模型

```
use App\Http\Model\Dbs;
```

下面是简单的查找与更新

```
public function index()
{
//        $users = Dbs::where('userid',1)->get();
//        dd($users);
        $users = Dbs::find(1);
        $users->username = '小爽';
        $users->update();
        dd($users);
}
```

ORM模型还有很多内容,具体查看手册和项目中实际应用.


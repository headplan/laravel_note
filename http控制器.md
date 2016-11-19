# HTTP控制器

### 基本控制器

**创建控制器**

```
<?php

namespace App\Http\Controllers;

class IndexController extends Controller
{
    public function index()
    {
        echo 'Hello World';
    }
}
```

也可以使用artisan命令创建控制器

```
php artisan make:controller HomeController
```

**注册路由**

在路由文件中注册控制器的路由.

```
Route::get('/', 'IndexController@index');
```

**控制器&命名空间&注册路由**

控制器创建在`Controllers\Admin`文件夹下`HomeControllers.php`

命名空间需要添加

```
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\;

use Illuminate\Http\Request;

use App\Http\Requests;

class HomeController extends Controller
{
    public function index()
    {
        echo '后台首页';
    }
}
```


# 手机注册验证码

#### 修改数据结构

在users表中增加phone字段 , 然后修改email字段为nullable .

```
php artisan make:migration add_phone_to_users_table --table=users
```

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class AddPhoneToUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('phone')->nullable()->unique()->after('name');
            $table->string('email')->nullable()->change();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropColumn('phone');
            $table->string('email')->nullable(false)->change();
        });
    }
}
```

添加修改MySQL字段属性需要的扩展包

```
composer require doctrine/dbal
```

然后运行迁移命令

```
php artisan migrate
```

#### 构建短信验证码接口

**创建api基类**

```
php artisan make:controller Api/Controller
```

添加Dingo提供的处理接口响应的trait

```php
<?php

namespace App\Http\Controllers\Api;

use Illuminate\Http\Request;
use Dingo\Api\Routing\Helpers;
use App\Http\Controllers\Controller as BaseController;

class Controller extends BaseController
{
    use Helpers;
}
```




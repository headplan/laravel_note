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

添加接口版本目录 , v1 , v2 .

**新增路由**

```php
<?php

use Illuminate\Http\Request;

$api = app('Dingo\Api\Routing\Router');

$api->version('v1', [
    'namespace' => 'App\Http\Controllers\Api\v1'
], function($api) {
    // 短信验证码
    $api->post('verificationCodes', 'VerificationCodesController@store')
        ->name('api.verificationCodes.store');
});
```

这里的namespace指向前面新建的目录v1 .

**创建短信验证控制器**

```
php artisan make:controller Api/v1/VerificationCodesController
```

修改一下基类的继承 , 添加store方法

```php
public function store()
{
    return $this->response->array(['test_message' => 'store verification code']);
}
```

利用 DingoApi 的 Helpers trait , 我们可以使用`$this->response->array`返回一个测试用的响应 . 测试一下接口 . 

**创建 API 表单请求验证类**

```
php artisan make:request Api/VerificationCodeRequest
```

```php
<?php

namespace App\Http\Requests\Api;

use Dingo\Api\Http\FormRequest;

class VerificationCodeRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'phone' => [
                'required',
                'regex:/^((13[0-9])|(14[5,7])|(15[0-3,5-9])|(17[0,3,5-8])|(18[0-9])|166|198|199|(147))\d{8}$/',
                'unique:users'
            ]
        ];
    }
}
```




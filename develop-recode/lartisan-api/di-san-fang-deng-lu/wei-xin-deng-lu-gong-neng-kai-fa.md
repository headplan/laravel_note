# 微信登录功能开发

#### 调整用户表结构

首先需要为 users 表增加两个字段 , weixin\_openid , weixin\_unionid , 用来记录微信用户的唯一标识 . 修改 password 字段为 nullable , 因为第三方登录不需要密码 .

```
php artisan make:migration add_weixin_openid_to_users_table --table=users
```

编辑迁移文件 :

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class AddWeixinOpenidToUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('weixin_openid')->unique()->nullable()->after('password');
            $table->string('weixin_unionid')->unique()->nullable()->after('weixin_openid');
            $table->string('password')->nullable()->change();
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
            $table->dropColumn('weixin_openid');
            $table->dropColumn('weixin_unionid');
            $table->string('password')->nullable(false)->change();
        });
    }
}
```

运行迁移

```
php artisan migrate
```

#### 路由设计

为了区分用户账号密码登录和第三方登录 , 将登录接口设计为2个 .

**添加第三方登录接口**

```php
# 第三方登录
$api->post('socials/{social_type}/authorizations', 'AuthorizationsController@socialStore')
    ->name('api.socials.authorizations.store');
```

**第三方登录逻辑**

创建 controller 和 request

```
$ php artisan make:controller Api/v1/AuthorizationsController
$ php artisan make:request Api/SocialAuthorizationRequest
```

修改验证规则

```php
public function rules()
{
    $rules = [
        'code' => 'required_without:access_token|string',
        'access_token' => 'required_without:code|string',
    ];

    # 微信还需要验证openid
    if ($this->social_type == 'weixin' && !$this->code) {
        $rules['openid']  = 'required|string';
    }
    
    return $rules;
}
```

修改模型的fillable字段

```
'weixin_openid', 'weixin_unionid'
```

编写逻辑 : 

```

```

**使用postman调试接口**

```
http://lartisan.bbs/api/socials/:social_type/authorizations
```

这里的url中:social\_type是一个postman的变量 , 可以自定义设置 , 这里为`weixin` . 


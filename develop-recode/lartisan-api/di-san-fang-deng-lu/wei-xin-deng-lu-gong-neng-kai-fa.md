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




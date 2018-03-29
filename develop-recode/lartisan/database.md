# 数据

**创建分支**

```
git checkout master
git checkout -b database
```

#### 数据库迁移

> 迁移就像是数据库中的版本控制，它让团队成员之间能够轻松的修改跟共享应用程序的数据库结构，而不用担心并行更新数据结构而造成冲突等问题 .
>
> Migration 的建表方法大部分情况下能兼容 MySQL, PostgreSQL, SQLite 甚至是 Oracle 等主流数据库系统。
>
> 多人并行开发 :
>
> * 代码版本管理；
> * 数据库版本控制 —— 如：回滚/重置/更新等；
> * 兼容多种数据库系统；
> * 部署方便。

在`database/migrations`中 , 已经准备了两个默认迁移文件 . 迁移文件中有两个方法 :

* up\(\)方法运行迁移,创建Schema::create\(\)表
* down\(\)方法回滚迁移,删除Schema::dropIfExists\(\)表

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('email')->unique();
            $table->string('password', 60);
            $table->rememberToken();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('users');
    }
}
```

**相关文档**

[https://laravel-china.org/docs/laravel/5.5/migrations\#creating-tables](https://laravel-china.org/docs/laravel/5.5/migrations#creating-tables)

#### 常用迁移命令

```
php artisan migrate # 运行数据库迁移
migrate:fresh       # 删除所有表并重新运行所有迁移,物理删除数据库中所有表,再运行迁移
migrate:install     # 创建迁移存储库,在数据库生成migrations表,记录迁移状态
migrate:refresh     # 重置并重新运行所有迁移,回滚之后再运行迁移
migrate:reset       # 回滚所有数据库迁移,清空数据,删除表.迁移表数据删除,索引保留
migrate:rollback    # 回滚上次数据库迁移,清空数据,删除表.迁移表数据删除,索引保留
migrate:status      # 显示每个迁移的状态
php artisan make:migration # 创建迁移文件
```

#### 更新env数据库相关配置

配置offline.evn和online.env文件 .


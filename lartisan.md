# Lartisan社区

### 安装扩展包

```markdown
# 基于Laravel_Basic创建
composer create-project laravel/laravel ./ "5.4.*" --prefer-dist(强制使用压缩包,而不是克隆源代码)
# 包含下面3个包
composer require 'barryvdh/laravel-debugbar' --dev
composer require 'barryvdh/laravel-ide-helper' --dev
composer require 'summerblue/generator' --dev
=====git commit
# 修改composer.json和基本配置
"name": "headplan/lartisan",
"description": "Lartisan.",
"keywords": ["lartisan", "headplan"],
'name' => 'Lartisan',
'timezone' => 'PRC',
# 配置上面三个包(Laravel_Basic已经更新)
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider" // 生成配置
php artisan ide-helper:generate // 生成代码对应文档
php artisan make:scaffold Projects --schema="name:string:index,description:text:nullable,subscriber_count:integer:unsigned,default(0)" // 需要时再使用
# 重新生成key
php artisan key:generate
# 生成认证
php artisan make:auth
=====git commit
# 引入权限管理包
composer require 'Zizaco/Entrust'
配置Entrust包
配置数据库表前缀
=====git commit
# 配置前后台分离
config/auth.php
# 创建后台用户模型,参数-m自动创建迁移文件,迁移文件和users刚生成的一样
php artisan make:model Models/Dashbord -m
Schema::create('dashboards', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
# 创建给Dashbord填充数据的Seeder
php artisan make:seeder DashboardsTableSeeder
# 编辑工厂ModelFactory.php文件
$factory->define(App\Models\Dashboard::class, function (Faker\Generator $faker) {
    static $password;
    return [
        'name' => $faker->name,
        'email' => $faker->safeEmail,
        'password' => $password ?: $password = bcrypt('secret'),
        'remember_token' => str_random(10),
    ];
});
# 在***TableSeeder.php中填充数据
factory('App\Models\Admin',3)->create([
    'password' => bcrypt('123456')
]);
# 在DatabaseSeeder.php中call上
$this->call(***TableSeeder::class);
# 生成
php artisan migrate:reset
php artisan migrate --seed
=====git commit:前后台分离基本配置
# 修改Dashboard模型类,现在和User一样
# 后台用户认证路由及控制器
php artisan make:controller Dashboard/LoginController 
php artisan make:controller Dashboard/DashboardController
```




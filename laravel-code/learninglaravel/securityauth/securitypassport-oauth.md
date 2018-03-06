# Security\_Passport\_OAuth

Laravel 项目中可以使用 Passport 轻而易举地实现 API 授权过程 , Passport 基于 [League OAuth2 server](https://github.com/thephpleague/oauth2-server) 实现 , 该项目的维护人是[Alex Bilbie](https://github.com/alexbilbie) .

#### 安装

```
composer require laravel/passport
```

将 Passport 的服务提供者注册到配置文件`config/app.php`的`providers`数组中 :

```
Laravel\Passport\PassportServiceProvider::class,
```

前面的内容完成后 , 运行迁移命令 , 会自动创建应用程序需要的客户端数据表和令牌数据表 :

```
php artisan migrate
```

> 如果不打算使用 Passport 的默认迁移 , 应该在`AppServiceProvider`的`register`方法中调用`Passport :: ignoreMigrations`方法 , 可以导出这个默认迁移用
>
> `php artisan vendor:publish --tag=passport-migrations`
>
> 命令
>
> ```
> # AppServiceProvider
> use Laravel\Passport\Passport;
> public function register()
> {
>     Passport::ignoreMigrations();
> }
> ```

接下来，你需要运行`passport:install`命令来创建生成安全访问令牌时用到的加密密钥，同时，这条命令也会创建「私人访问」客户端和「密码授权」客户端 :

```
php artisan passport:install
```

然后 , 请将`Laravel\Passport\HasApiTokens`Trait 添加到`App\User`模型中，这个 Trait 会给你的模型提供一些辅助函数，用于检查已认证用户的令牌和使用作用域 :

```php
<?php

namespace App;

use Laravel\Passport\HasApiTokens;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}
```

接下来 , 需要在`AuthServiceProvider`的`boot`方法中调用`Passport::routes`函数 , 这个函数会注册一些在访问令牌、客户端、私人访问令牌的发放和吊销过程中会用到的必要路由 :

```php
<?php

namespace App\Providers;

use Laravel\Passport\Passport;
use Illuminate\Support\Facades\Gate;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * The policy mappings for the application.
     *
     * @var array
     */
    protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
    ];

    /**
     * Register any authentication / authorization services.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerPolicies();

        Passport::routes();
    }
}
```

最后，需要将配置文件`config/auth.php`中`api`部分的授权保护项（`driver`）改为`passport`。此调整会让你的应用程序在接收到 API 的授权请求时使用 Passport 的`TokenGuard`来处理 :

```
'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],

    'api' => [
        'driver' => 'passport',
        'provider' => 'users',
    ],
],
```

**配置完成 . **

---

使用seed来预设填充数据 , 先创建一个Seeder

```
php artisan make:seeder UsersTableSeeder
```

在run方法中添加数据

```
DB::table('users')->insert([
'name'=>'admin',
'email'=>'godcheese@gioov.com',
'password'=>bcrypt('admin')
]);
```

跑数据

```
php artisan db:seed --class=UsersTableSeeder
```

---

#### 使用Vue组件

先安装组件 , 其实是移动

```
php artisan vendor:publish --tag=passport-components
```

已发布的组件将被放置在`resources/assets/js/components`目录中 , 可以在`resources/assets/js/app.js`文件中注册这些已发布的组件 :

```js
Vue.component(
    'passport-clients',
    require('./components/passport/Clients.vue')
);

Vue.component(
    'passport-authorized-clients',
    require('./components/passport/AuthorizedClients.vue')
);

Vue.component(
    'passport-personal-access-tokens',
    require('./components/passport/PersonalAccessTokens.vue')
);
```

然后 , 就可以直接将这些组件直接放入应用程序的模板中，用于创建客户端和私人访问令牌 :

```
<passport-clients></passport-clients>
<passport-authorized-clients></passport-authorized-clients>
<passport-personal-access-tokens></passport-personal-access-tokens>
```

> 这里使用Vue的部分内容 , 还记得么 , npm run dev , 标签内容都放到&lt;div id="app"&gt;&lt;/div&gt;标签中 .

---

#### 配置

**令牌的有效期**

> 这里的两个有效期就是OAuth2中 , 我们提到的`token`和`refresh_token`

默认情况下，Passport 发放的访问令牌是永久有效的，不需要刷新。但是如果你想给访问令牌配置一个短一些的有效期，那你就需要用到`tokensExpireIn`和`refreshTokensExpireIn`方法了，上述两个方法同样需要在`AuthServiceProvider`的`boot`方法中调用 : 

```php
# 记住要加载Carbon类
use Carbon\Carbon;

/**
 * Register any authentication / authorization services.
 *
 * @return void
 */
public function boot()
{
    $this->registerPolicies();

    Passport::routes();

    Passport::tokensExpireIn(Carbon::now()->addDays(15));

    Passport::refreshTokensExpireIn(Carbon::now()->addDays(30));
}
```




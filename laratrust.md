# Laratrust

[https://github.com/santigarcor/laratrust](https://github.com/santigarcor/laratrust)

[http://laratrust.readthedocs.io/en/4.0/](http://laratrust.readthedocs.io/en/4.0/)

#### **安装**

```
composer require "santigarcor/laratrust:4.0.*"
```

**配置config/app.php\(如果是Laravel5.5版本则不需要\)**

```
# providers
Laratrust\LaratrustServiceProvider::class,

# aliases
'Laratrust'   => Laratrust\LaratrustFacade::class,
```

**生成配置文件**

```
php artisan vendor:publish --tag="laratrust"
```

> 如果生成失败执行清空即可
>
> ```
> php artisan config:clear
> ```

**配置中间件**

```
# routeMiddleware
'role' => \Laratrust\Middleware\LaratrustRole::class,
'permission' => \Laratrust\Middleware\LaratrustPermission::class,
'ability' => \Laratrust\Middleware\LaratrustAbility::class,
```

#### 配置

包的配置文件在**config/laratrust.php**中 , 其中包含所有Laratrust的配置

**多态关联 - use\_morph\_map**

在模型之间的关系中使用MorphMap . 设置为true , 则将使用morphMap功能 , 多态关联使用的是"user\_models"数组值 .

```
use_morph_map => true
```

**团队功能 - use\_teams**

团队功能也是可选的 , 配置文件

```
use_teams => true
```

> 如果开启团队功能 , 还需要执行两条命令
>
> ```
> php artisan laratrust:setup-teams
> php artisan migrate
> ```

**用户模型 - user\_models**

这里的配置就是用户模型信息的数组 , 如果开启前面的多态开启了 , 也表示多态的关联信息 , 例如包含了多个用户模型信息 , 有前台后台用户区分的角色权限管理 , 权限表中的user\_type字段存储的是这里配置的类名 , 开启多态 , 就可以用key名存储了 .

```
'user_models' => [
    'users' => 'App\User',
],
```

**自动设置**

可以使用下面的命令让Laratrust自动设置 ,

```
php artisan laratrust:setup
```

前提是别忘记配置服务提供者在config/app.php中 .

这条命令会生成迁移文件和模型 :

* database/migrations/0000\_laratrust\_setup\_tables.php - 生成迁移文件
* app/Permission.php - 生成模型
* app/Role.php - 生成模型
* app/Team.php - 如果配置中启用了团队功能 , 则生成此模型
* app/User.php - 修改User模型\(就是config/laratrust.php中配置的模型\) , 添加use LaratrustUserTrait

> 生产必备
>
> ```
> composer dump-autoload
> ```




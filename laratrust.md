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

> 如果开启团队功能 , 还需要执行两条命令 , 这里是单独安装的时候执行的 .
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

* **database/migrations/&lt;timestamp&gt;\_laratrust\_setup\_tables.php** - 生成迁移文件
* **app/Permission.php** - 生成模型
* **app/Role.php** - 生成模型
* **app/Team.php** - 如果配置中启用了团队功能 , 则生成此模型
* **app/User.php** - 修改User模型\(就是config/laratrust.php中配置的模型\) , 添加use LaratrustUserTrait

> 生产必备
>
> ```
> composer dump-autoload
> ```

**生成迁移**

生成迁移文件

```
php artisan laratrust:migration
```

如果已经存在迁移文件\(上一步自动设置会生成\) , 直接执行

```
php artisan migrate
```

即可 . 如果开启了团队功能 , 会生成6张表 :

* roles - 存储角色记录
* permissions - 存储权限记录
* teams - 存储团队记录
* role\_user - 存储角色和用户之间的多态关系
* permission\_role - 存储权限和角色之间的多对多关系
* permission\_user - 存储权限和用户之间的多态关系

**配置模型 - models**

配置Laratrust用来定义角色 , 权限和团队的模型 , 可以配置不同的命名空间中或不同的名称 .

```
'models' => [
    /**
     * Role model
     */
    'role' => 'App\Role',

    /**
     * Permission model
     */
    'permission' => 'App\Permission',

    /**
     * Team model
     */
    'team' => 'App\Team',

],
```

**Role角色模型**

有三个主要的属性

* name - 角色的唯一名称 , 用于在应用程序层查找角色信息 . 例如admin , owner等 . 
* display\_name - 角色的显示名称 , 可选 , 不一定是唯一的 , 例如用户管理员 , 项目所有者等 . 
* description - 更详细地说明角色的作用 , 可选 . 

display\_name和description都是可选的 , 也就是说它们的字段在数据库中是可以为空的 .

**Permission权限模型**

也有三个主要的属性 , 而且和角色模型一样

* name - 权限的唯一名称 , 用于在应用程序层中查找权限信息 . 例如create-post , edit-user等 . 
* display\_name - 权限的显示名称 , 可选 , 不一定是唯一的 , 例如创建帖子 , 编辑用户等 . 
* description - 对权限更详细的解释 , 可选 . 

**Team团队模型**

也有三个主要的属性 , 同上

* name - 团队名称
* display\_name - 团队显示名
* description - 对权限更详细的解释




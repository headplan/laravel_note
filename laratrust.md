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

**多态关联**

在模型之间的关系中使用MorphMap . 设置为true , 则将使用morphMap功能 , 多态关联使用的是"user\_models"数组值 .

```
use_morph_map => true
```

**团队功能**

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




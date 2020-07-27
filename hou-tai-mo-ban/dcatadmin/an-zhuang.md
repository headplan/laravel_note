# 安装

#### 环境

* PHP &gt;= 7.1
* Laravel 5.5.0 ~ 7.\*
* Fileinfo PHP Extension

> 更换阿里云镜像
>
> ```
> composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
> ```

#### 安装Laravel

```
composer create-project --prefer-dist laravel/laravel 项目名称
```

安装完`laravel`之后需要设置数据库连接设置正确 .

```
composer require dcat/laravel-admin
```

然后运行下面的命令来发布资源 :

```
php artisan admin:publish
```

在该命令会生成配置文`config/admin.php` , 可以在里面修改安装的地址、数据库连接、以及表名 , 建议都是用默认配置不修改 .

> 执行这一步命令可能会报以下错误`Specified key was too long ... 767 bytes`，如果出现这个报错 , 请在`app/Providers/AppServiceProvider.php`文件的boot方法中加上代码`\Schema::defaultStringLength(191);`，然后删除掉数据库中的所有数据表 , 再重新运行一遍`php artisan admin:install`命令即可 .

启动服务后 , 在浏览器打开`http://localhost/admin/`, 使用用户名`admin`和密码`admin`登陆 .

#### 生成的文件

**配置文件**

所有的配置都在`config/admin.php`文件中 .

**后台项目文件**

后台的安装目录为`app/Admin`

```
app/Admin
├── Controllers
│   ├── ExampleController.php
│   └── HomeController.php
├── Metrics
│   └── Examples
│       ├── NewDevices.php
│       ├── NewUsers.php
│       ├── ProductOrders.php
│       ├── Sessions.php
│       ├── Tickets.php
│       └── TotalUsers.php
├── bootstrap.php
└── routes.php
```

* `app/Admin/routes.php`文件用来配置后台路由 . 
* `app/Admin/bootstrap.php`是`dcat-admin`的启动文件 , 使用方法请参考文件里面的注释 . 
* `app/Admin/Controllers`目录用来存放后台控制器文件 , 该目录下的`HomeController.php`文件是后台首页的显示控制器 , `ExampleController.php`为实例文件
* `app/Admin/Metrics/Examples`里面存放的是`数据统计卡片(Metric Card)`的示例代码 . 

**静态文件**

后台所需的前端静态文件在`/public/vendor/dcat-admin`目录下 . 

**数据表迁移文件**

对应的数据表迁移文件在`/database/migrations`目录下 . 

**语言包**

语言包文件在`/resources/lang`目录下


最近一周翻阅了网上很多关于Laravel框架的资料，像比较不错的社区 [Laravel China](https://laravel-china.org/)，以及开源项目聚集地 [Laravel学院](http://laravelacademy.org/laravel-project)，比较不错的文档 [Laravel 中文网](http://v3.golaravel.com/docs/artisan/commands.html)，最重要的可关注官方源代码[Github](https://github.com/laravel)有最新的动态和更新。

---

[想要快捷部署项目，优雅起来那就先用用composer神器吧 ](https://getcomposer.org/)

* `来一波官网最新下载地址`

```
wget https://getcomposer.org/download/1.3.2/composer.phar
```

* `配置一波` 

```
mv composer.phar /usr/local/bin/composer， chmod 775 composer
```

* `或者直接curl`

```
curl -sS http://install.phpcomposer.com/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

---

`初用Laravel对于Artisan控制台还是比较感兴趣的，方便快捷不是盖的。`

```
1.创建一个新的资源控制器

php artisan make:controller UserController

2.创建一个新的 Eloquent 模型类

php artisan make:model User

3.项目进入、退出维护模式 

php artisan down, php artisan up

4. 进行数据库迁移（migration）

php artisan migrate

5.创建article表结构

php artisan make:migration create_tableName_table
```

* `使用migration新建表结构`

```
1.php artisan make:migration create_demo_table
2.修改up方法
3.执行构建 php artisan migrate，demo里可以改为你想要的名字。完成后数据库里还会多了个migrations表，用来记录数据库迁移信息
```

---

* `关于Seeder`

`database/seeds下则对应相应的数据库改动信息，包含数据。Seeder 解决的是我们在开发 web 应用的时候，需要手动向数据库中填入假数据的繁琐低效问题。`

```
1.创建文件 php artisan make:seeder DemoSeeder
2.修改run方法
3.上面代码中的 \App\Demo 为命名空间绝对引用。接下来我们把 DemoSeeder 注册到系统内。修改 database/seeds/DatabaseSeeder.php 中的 run 函数为,加入一行： $this->call(DemoSeeder::class)
4.由于 database 目录没有像 app 目录那样被 composer 注册为 psr-4 自动加载，采用的是 psr-0 classmap 方式，所以我们还需要运行以下命令把 DemoSeeder.php 加入自动加载系统，避免找不到类的错误，执行 composer dump-autoload
5.最后执行：php artisan db:seed 这样就在Demo表中新增了初始化数据了
```

```
注：其他指令集参考: http://v3.golaravel.com/docs/artisan/commands.html
```

---

`一般安装现有项目的基本步骤`（以 [laravel-5-blog](https://github.com/yccphp/laravel-5-blog) 为例）

```
1. 构建项目：git clone git@github.com:yccphp/laravel-5-blog.git
2. cd laravel-5-blog/
composer install
3. 数据库配置：修改 .env.example 为 .env
4. 权限设置：sudo chmod o+w -R storage； sudo chmod o+x -R  bootstrap/cache/; sudo chown -R www-data:www-data public/uploads
5. 安装数据库：php artisan migrate
6. 填充数据：php artisan db:seed
```




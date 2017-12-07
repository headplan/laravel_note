# Laravel-ide-helper高效的IDE智能提示插件

扩展包能让你的 IDE \( PHPStorm, Sublime \) 实现自动完成、代码智能提示和代码跟踪等功能，大大提高你的开发效率。

#### 安装

```
composer require barryvdh/laravel-ide-helper
```

安装完成后，在`config/app.php`添加以下内容到providers数组

```
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
```

**生成Laravel Facades对应文档**

```
php artisan ide-helper:generate
```

> 注 : 要先清除bootstrap/compiled.php文件 , 可以执行`php artisan clear-compiled`命令 ,

也可以配置composer.json文件 , 每次提交自动执行 :

```js
"scripts":{
    "post-update-cmd": [
        "Illuminate\\Foundation\\ComposerScripts::postUpdate",
        "php artisan ide-helper:generate",
        "php artisan ide-helper:meta",
        "php artisan optimize"
    ]
},
```

测试效果 , 配置路由文件web.php , 写Facades的时候`Route::`  IDE会自动提示 .

**Laravel-ide-helper的配置文件**

```
php artisan vendor:publish --provider="Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider" --tag=config
```

生成\(复制\)配置`ide-helper.php`文件到`config`文件夹 . 配置文件可以更改和设置接口和特定类等 . 

生成器会尝试定位真正的类 , 如果找不到 , 可以在配置文件中定义 . 

有些类需要数据库连接 , 如果没有相应的数据库连接 , 某些门面可能无法包含到生成器文件中 . 可以通过`-M`参数 , 使用内存SQLite驱动 . 还可以配置要包含的辅助函数文件 , 默认该选项并未开启 , 可以通过`--helpers`选项或者`-H`覆盖默认配置 , 也可以在配置文件中添加自定义的辅助函数文件 . 

**ide-helper.php : **

```
<?php
return array(
# 默认的文件名(不带扩展名)
# 默认的格式(php或json)
'filename'  => '_ide_helper',
'format'    => 'php',
```

    # 设置为true,生成常用的Fluent方法(也就是常说的方法链,比如设计模式中的流接口模式Fluent Interface)
    'include_fluent' => true,
    # 设置为true会在_ide_helper文件中添加下面的内容
    > namespace Illuminate\Support {
    >     /**
    >      * Methods commonly used in migrations
    >      *
    >      * @method Fluent after(string $column) Add the after modifier
    >      * @method Fluent charset(string $charset) Add the character set modifier
    >      * @method Fluent collation(string $collation) Add the collation modifier
    >      * @method Fluent comment(string $comment) Add comment
    >      * @method Fluent default(mixed $value) Add the default modifier
    >      * @method Fluent first() Select first row
    >      * @method Fluent index(string $name = null) Add the in dex clause
    >      * @method Fluent on(string $table) `on` of a foreign key
    >      * @method Fluent onDelete(string $action) `on delete` of a foreign key
    >      * @method Fluent onUpdate(string $action) `on update` of a foreign key
    >      * @method Fluent primary() Add the primary key modifier
    >      * @method Fluent references(string $column) `references` of a foreign key
    >      * @method Fluent nullable() Add the nullable modifier
    >      * @method Fluent unique(string $name = null) Add unique index clause
    >      * @method Fluent unsigned() Add the unsigned modifier
    >      * @method Fluent useCurrent() Add the default timestamp value
    >      */
    >     class Fluent {}
    > }




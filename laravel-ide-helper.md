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

#### **生成Laravel Facades对应文档**

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

#### **生成模型对应的文档**

在使用本特性之前 , 需要先引入依赖

```
composer require doctrine/dbal
```

如果不自己写文档属性 , 可以使用命令来基于数据表字段,关联关系以及getters/setters生成对应的phpDoc文档 : 

```
php artisan ide-helper:models
```

执行命令会提示是否要改写现有的模型文件 , 还是以写入`_ide_helper_models. php`文件代替 , 默认为no , 生成`_ide_helper_models. php`文件 . 可以使用`-W`参数 , 直接写入模型文件中 , 或者`-N`参数生成`_ide_helper_models. php`文件 . 

```
php artisan ide-helper:models -W
php artisan ide-helper:models -N
```




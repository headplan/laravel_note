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

---

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

需要注意的是在进行操作之前需要备份模型文件 , 因为我们要保留之前已存在的 , 只是追加新属性和方法 , 而不是覆盖和重写 . phpdoc文档已存在会被替换 , 否则新增 . 我们通过`—reset(-R)`参数 , 将已存在的phpdoc忽略 , 只保存新增的字段/关系 .

默认情况`app/models`目录下的模型会被遍历 , 也可以指定模型或者目录 :

```
php artisan ide-helper:models Post User
php artisan ide-helper:models --dir="path/to/models" --dir="app/src/Model"
```

> 也可以在配置文件中设置模型目录

使用命名空间也可以 , 但要注意添加引号 :

```
php artisan ide-helper:models "Api\User"
# 或者使用双斜线
php artisan ide-helper:models Api\\User
```

使用`--ignore(-I)`选项来忽略模型文档生成 :

```
php artisan ide-helper:models --ignore="Post,User"
```

为了正确识别模型方法 , 例如`paginate, findOrFail`等 , 可以让模型类继承`Eloquent`或者给模型类添加注释 , 就可以自动提示了 :

```
/**
 * @mixin \Eloquent
 */
```

---

#### 生成PHPStorm中容器实例对应的Meta

可以生成一个PHPStorm meta文件来添加工厂设计模式支持，对Laravel而言，这意味着我们可以让PHPStorm理解从IoC容器中取出的对象类型 . 例如 , `events`会返回`Illuminate\Events\Dispatcher`对象 , 因此通过meta文件你可以调用`app('events')`然后它会自动补全对应的调度方法 .

```
php artisan ide-helper:meta
```

```
app('events')->fire();
\App::make('events')->fire();

/** @var \Illuminate\Foundation\Application $app */
$app->make('events')->fire();

// When the key is not found, it uses the argument as class name
app('App\SomeClass');
```

生成文档之后需要重启PHPStorm , 目的也是为了重新索引.phpstorm.meta.php文件 . 需要注意的是 , 当因为未找到类而产生致命异常时 , 可以检查一下配置文件 , 例如没有配置S3云服务而引入了 , 没有使用Redis , 而引入了服务提供者 , 可能发生致命异常 . 

为了方便 , 可以直接添加预生成文件 : 

```
https://gist.github.com/barryvdh/bb6ffc5d11e0a75dba67
```




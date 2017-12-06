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

**生成Laravel Facades代码对应文档**

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




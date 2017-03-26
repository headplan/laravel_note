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

接下来运行以下命令生成代码对应文档

```
php artisan ide-helper:generate
```




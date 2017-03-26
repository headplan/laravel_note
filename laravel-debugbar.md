# Laravel-debugbar开发调试器

#### 安装

```
composer require barryvdh/laravel-debugbar
```

追加到config/app.php中

```
'providers' => [
    ...
    Barryvdh\Debugbar\ServiceProvider::class,
],
```

同时在`aliases`数组内追加如下内容

```
'aliases' => [
    ...
    'Debugbar' => Barryvdh\Debugbar\Facade::class,
]
```




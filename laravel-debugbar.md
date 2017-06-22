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

运行以下命令生成此扩展包的配置文件`config/debugbar.php`

```
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```

打开`config/debugbar.php`, 将`enabled`的值设置为

```
'enabled' => env('APP_DEBUG', false),
```

修改完以后, Debugbar 分析器的启动状态将由`.env`文件中`APP_DEBUG`值决定.或者直接默认即可

```
'enabled' => env('DEBUGBAR_ENABLED', null),
```




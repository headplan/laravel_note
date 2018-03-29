# 静态页面

**创建分支**

```
$ git checkout master
$ git checkout -b static-pages
```

**删除无用的视图**

```
rm resources/views/welcome.blade.php
```

**创建路由**

_routes/web.php_

```
<?php
h
Route::get('/', 'StaticPagesController@home');
Route::get('/help', 'StaticPagesController@help');
Route::get('/about', 'StaticPagesController@about');
```

**创建控制器**

```
$ php artisan make:controller StaticPagesController
```

**添加控制器方法**

```
...
public function about()
{
    return view('static_pages/about');
}
```

**添加视图文件**

```
touch home.blade.php
touch help.blade.php
touch about.blade.php
```




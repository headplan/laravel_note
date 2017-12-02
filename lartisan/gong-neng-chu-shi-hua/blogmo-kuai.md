# Blog模块

前面的数据初始化例子添加了基本的数据迁移 . 这里初始化基本功能 . 

#### 配置文件

创建Blog配置文件 : `config/blog.php`

```
return [
    'title' => 'Lartisan Blog',
    'posts_per_page' => 5
];
```

#### 路由与控制器

```
Route::get('/', function () {
    return redirect('blog');
});
Route::get('/blog', 'BlogController@index')->name('blog');
Route::get('/blog/{slug}', 'BlogController@show');
```

```
php artisan make:controller BlogController
```

#### 创建视图

```
views/blog/index.blade.php
views/blog/show.blade.php
```






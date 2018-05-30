# 专题模块

生成数据迁移以及基本内容

```
php artisan make:migration create_article_categories_table
php artisan make:migration create_article_topics_table
```

```
# 修改头部入口链接,没有专栏默认进入创建专栏页,有则进入添加内容页.
专栏路由:articles/columns
添加创建专栏页面,路由
添加编辑专栏页面,路由
添加显示专栏页面,路由
```

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

---




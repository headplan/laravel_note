# Change

#### change 1

把nav部分从layout中拆分出来到include , 然后在layout中添加了两个堆栈

**@stack**

用来覆盖或添加引入文件时使用 . 可以使用@push和@prepend添加 .

#### change 2

添加view和sass文件

```
# side-menu的样式
resources/assets/sass/manage.scss
# 左边栏模板
resources/views/_includes/nav/manage.blade.php
# manage的layout
resources/views/layouts/manage.blade.php
# dashboard模板
resources/views/manage/dashboard.blade.php
```

**修改view和sass文件**

```
# 添加dashboard链接
resources/views/_includes/nav/main.blade.php
# import manage.scss文件
resources/assets/sass/app.scss
```

**添加路由和控制器**

```
# 添加dashboard方法和index,并且index跳转manage.dashboard
app/Http/Controllers/ManageController.php
# 添加路由并且添加权限中间件
routes/web.php
Route::group(['prefix'=>'manage','middleware'=>'role:superadministrator|administrator'], function () {
    Route::get('/', 'ManageController@index');
    Route::get('/dashboard', 'ManageController@dashboard')->name('manage.dashboard');
});
```

**修改config/laratrust.php配置**

```
配置'middleware'为跳转到指定页面
```




# 用户模块

#### 注册登录

生成代码 :

```
php artisan make:auth
```

`make:auth`会询问我们是否要覆盖`app.blade.php`这里选择no

可以使用`git status`查看文件更改状态 .

查看新增的路由 :

```
php artisan route:list
```

删除生成的多余内容 : 

```
# 多余的路由绑定
Route::get('/home', 'HomeController@index')->name('home');
# 多余的控制器和模板
$ rm app/Http/Controllers/HomeController.php
$ rm resources/views/home.blade.php
```




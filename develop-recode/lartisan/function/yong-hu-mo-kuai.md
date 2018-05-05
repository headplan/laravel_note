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

删除修改生成的多余内容 :

```
# 多余的路由绑定
Route::get('/home', 'HomeController@index')->name('home');
# 多余的控制器和模板
$ rm resources/views/home.blade.php
# 控制修改回原来加载的模板
return view('home.index');
# 中间件暂时排除index
->except('index');
```




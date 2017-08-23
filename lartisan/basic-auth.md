# Basic Auth

目前角色权限控制使用了Laratrust包 , 现在配置也是基本的安装 , 可以参考笔记中用户认证的Laratrust包相关内容 . 具体权限分配暂时使用默认状态 .

安装了Laratrust包之后提交过一次 , 还要写一些关于操作Laratrust的功能 , 都是基本的操作 , CRUD等等 . 

```
php artisan make:controller UserController --resource
```

创建Resource控制器 , 添加路由

```
Route::resource('/users', 'UserController');
```

然后开始编辑这些和User相关的文件 . 这里的翻页模板需要修改 , 运行命令

```
php artisan vendor:publish --tag=laravel-pagination
```

上面的命令的意思是移动视图文件到 

```
/vendor/laravel/framework/src/Illuminate/Pagination/resources/views
to
/resources/views/vendor/pagination
```

这样就可以编辑翻页视图的模板了 . 


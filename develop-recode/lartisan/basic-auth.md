# Basic Auth

目前角色权限控制使用了Laratrust包 , 现在配置也是基本的安装 , 可以参考笔记中用户认证的Laratrust包相关内容 . 具体权限分配暂时使用默认状态 .

**User相关**

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

基本的模板

```
resources/views/manage/users/create.blade.php
resources/views/manage/users/edit.blade.php
resources/views/manage/users/index.blade.php
resources/views/manage/users/show.blade.php
```

继承自

```
resources/views/layouts/manage.blade.php
```

这里修改了vue的初始化 , 删除了js/app.js中的默认初始化 . 没有使用app变量名 , 否则会冲突 . 删除了这里

```
// Vue.component('example', require('./components/Example.vue'));
const app = new Vue({
    el: '#app',
    data: {}
});
```

**Permission相关**

创建路由

资源路由 , 排除destroy销毁方法 .

```
Route::resource('/permissions', 'PermissionController', ['except' => 'destroy']);
```

创建控制器

```
php artisan make:controller PermissionController --resource
```

和对应的方法以及视图文件 . 

这里简单实用Buefy的模块 . 


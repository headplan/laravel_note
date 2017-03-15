# Laravel Forms 使用

### **创建内容**

**注册路由**

```
Route::get('/articles/create','ArticlesController@create');
```

**编辑控制器**

**创建视图**

这里引入illuminate/html这个包

```
composer require illuminate/html
```

> Laravel 5.4 已经更名为laravelcollective/html

引入之后需要简单配置

```
config/app.php
下的服务提供者中引入刚刚的类
Illuminate\Html\HtmlServiceProvider::class,
在快捷方式中指定门面类
'Form' => Illuminate\Html\FormBuilder::class,
```




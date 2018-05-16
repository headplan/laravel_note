# 分类模块

```
# 创建分类模型,并创建迁移文件
php artisan make:model Models/Category -m
# 编辑迁移文件,生成分类表
# 添加迁移默认数据文件,在迁移文件中直接写插入数据,作为默认数据
php artisan make:migration seed_categories_data
# 生成php artisan migrate

# 添加分类路由
Route::resource('categories', 'CategoriesController', ['only' => ['show']]);
# 创建控制器
php artisan make:controller CategoriesController

# 与视图共享数据可以在boot中使用
View::share('key', 'value');的方式.这里使用官方的第二种方式.
# 视图合成器
创建新的Providers:php artisan make:provider ComposerServiceProvider
在配置文件config/app.php中添加注册地址
App\Providers\ComposerServiceProvider::class,
创建文件夹app\Http\ViewComposers管理视图合成器,
创建前台视图合成器HomeComposer类.用来整合一些共享数据.先在服务中绑定
View::composer(
    'layouts.header', 'App\Http\ViewComposers\HomeComposer'
);
绑定中可以使用*匹配目录.然后编辑HomeComposer类,类中可以注入一些数据
这里的数据暂时不用Repository层.-------------------------------------------->todo
在Model中创建scope方法,写入给HomeComposer调用.这里只take(4)条数据
```




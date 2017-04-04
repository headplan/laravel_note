# 视图

### 知识点整理

```
1.创建视图
    resources/views/ # 视图存放目录
    全局辅助函数view('admin.greeting', ['name' => 'James']);
    第一个参数用"."引用嵌套视图,第二个参数传递变量到视图
    判断视图是否存在
    if (view()->exists('emails.customer')) {
        // 调用不带参数的view(),返回一个Illuminate\Contracts\View\Factory实例,可以工厂上的所有方法
    }

2.传递数据到视图
    return view('greetings', ['name' => 'Victoria']);
    $view = view('greeting')->with('name', 'Victoria');
    在视图间共享数据
    应用视图工厂的share方法
    可以将其添加到AppServiceProvider或生成独立的服务提供者来存放它们
    public function boot()
    {
        view()->share('key', 'value');
    }

3.视图Composer
    视图Composer是当视图被渲染时的回调或类方法.
    如果你有一些数据要在视图每次渲染时都做绑定,
    可以使用视图Composer将逻辑组织到一个单独的地方.
    首先要在服务提供者中注册视图Composer.
    使用辅助函数 view 来访问 Illuminate\Contracts\View\Factory 的底层实现
    
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;
use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    /**
     * 在容器中注册绑定.
     * @return void
     */
    public function boot()
    {
        // 使用基于类的composers...
        View::composer(
            'profile', 'App\Http\ViewComposers\ProfileComposer'
        );

        // 使用基于闭包的composers...
        View::composer('dashboard', function ($view) {});
    }

    /**
     * 注册服务提供者.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}

# 注意:需要添加该服务提供者到配置文件 config/app.php 的 providers 数组中

现在我们已经注册了 Composer，每次 profile 视图被渲染时都会执行 ProfileComposer@compose，接下来我们来定义该 Composer 类

<?php

namespace App\Http\ViewComposers;

use Illuminate\View\View;
use Illuminate\Repositories\UserRepository;

class ProfileComposer
{
    /**
     * 用户仓库实现.
     *
     * @var UserRepository
     */
    protected $users;

    /**
     * 创建一个新的属性composer.
     *
     * @param UserRepository $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        // Dependencies automatically resolved by service container...
        $this->users = $users;
    }

    /**
     * 绑定数据到视图.
     *
     * @param View $view
     * @return void
     */
    public function compose(View $view)
    {
        $view->with('count', $this->users->count());
    }
}

视图被渲染前，Composer 类的 compose 方法被调用，同时 Illuminate\View\View 实例被注入该方法
从而可以使用其 with 方法来绑定数据到视图

添加 Composer 到多个视图
View::composer(
    ['profile', 'dashboard'],
    'App\Http\ViewComposers\MyViewComposer'
);
view()->composer('*', function ($view) {
    //
});

视图创建器
视图创建器和视图 Composer 非常类似
不同之处在于前者在视图实例化之后立即失效而不是等到视图即将渲染
使用 creator 方法即可注册一个视图创建器
View::creator('profile', 'App\Http\ViewCreators\ProfileCreator');
```




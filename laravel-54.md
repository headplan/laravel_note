# Laravel 5.4

> Laravel 5.4 在 5.3 的基础继续进行优化 : 在邮件和通知中支持Markdown、浏览器自动测试框架Laravel Dusk、Laravel Mix、Blade“组件”和“插槽”、在广播频道上进行路由模型绑定、在集合中支持高阶消息传递、基于对象的Eloquent事件、任务级别的“重试”和“超时”设置、“实时”门面、更好的支持Redis Cluster、自定义透视表（pivot）模型、请求输入清理中间件，等等。此外，官方开发组还review和重构了整个框架的底层代码，以让其更加干净和清晰。
>
> Github上的更新记录\(完整\) :
>
> [https://github.com/laravel/framework/blob/5.4/CHANGELOG-5.4.md](https://github.com/laravel/framework/blob/5.4/CHANGELOG-5.4.md)

#### 使用Markdown编写邮件和通知

该特性允许我们在邮件中利用预置模板和邮件通知组件，由于消息使用Markdown格式编写，因此Laravel可以将这些消息渲染成美观、响应式的HTML模板的同时自动为其生成纯文本副本。例如，下面是一个Markdown格式的邮件示例：

```
@component('mail::message')
# Order Shipped

Your order has been shipped!

@component('mail::button', ['url' => $url])
View Order
@endcomponent

Next Steps:

- Track Your Order On Our Website
- Pre-Sign For Delivery

Thanks,<br>
{{ config('app.name') }}
@endcomponent
```

利用这个简单的Markdown模板，Laravel就可以生成响应式的HTML邮件以及对应的纯文本副本：![](/assets/laravel-email.png)

> 你可以将所有Markdown邮件组件导出到自己的应用中进行自定义。要导出这些组件，可以使用Artisan命令`vendor:publish`来发布标签为`laravel-mail`的资源

#### Laravel Dusk

> Laravel Dusk为我们提供了优雅的、易于使用的浏览器自动化测试API。默认情况下，Dusk不需要在机器上安装JDK或Selenium，取而代之的，Dusk使用一种独立的[ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/home)安装方式。不过，你也可以使用其它兼容Selenium驱动的方式进行安装。
>
> 由于Dusk在操作过程中使用了真实的浏览器，所以可以很轻松地对那些重度使用JavaScript的应用进行测试和交互 .

#### Laravel Mix

> Laravel Mix是Laravel Elixir的精神继承者，完全基于Webpack而不是Gulp。Laravel Mix为使用通用CSS和JavaScript预处理器定义Laravel应用的Webpack构建步骤提供了流式API。通过简单的方法链，你可以定义流式资源管道，例如：
>
> ```
> mix.js('resources/assets/js/app.js', 'public/js')
>    .sass('resources/assets/sass/app.scss', 'public/css');
> ```

#### Blade组件&插槽

Blade组件和插槽为section和layout提供了类似的好处，不过，有些人可能会觉得组件和插槽的构思模型更容易理解 . 这和Vue中的组件概念类似 .

#### 广播中的模型绑定

和HTTP路由一样，频道路由现在也可以显式或隐式进行模型绑定，例如，我们可以通过请求一个实际的`Order`模型实例来取代之前获取字符串或数字订单ID：

```
use App\Order;

Broadcast::channel('order.{order}', function ($user, Order $order) {
    return $user->id === $order->user_id;
});
```

#### 集合高阶消息传递

集合现在支持“高阶消息传递”，从而精简对集合的操作，目前支持高阶消息传递的集合方法有：`contains`、`each`、`every`、`filter`、`first`、`map`、`partition`、`reject`、`sortBy`、`sortByDesc`和`sum`。

每一个高阶消息传递都可以通过集合实例上的动态属性进行访问，例如，我们使用`each`在集合中的每个对象上调用某个方法：

```
$users = User::where('votes', '>', 500)->get(); 
$users->each->markAsVip();
```

类似的，我们还可以通过`sum`在用户集合中聚合所有投票数：

```
$users = User::where('group', 'Development')->get();
return $users->sum->votes;
```

#### 基于对象的Eloquent事件

Eloquent事件处理器现在可以被映射到事件对象上，这为我们处理Eloquent事件并让其变得易于测试提供了一种直观的方式。开始之前，我们现先在Eloquent模型类中定义一个`events`属性数组，在这个数组中我们定义了模型生命周期的某些点与对应事件类的映射关系：

```
<?php

namespace App;

use App\Events\UserSaved;
use App\Events\UserDeleted;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * The event map for the model.
     *
     * @var array
     */
    protected $events = [
        'saved' => UserSaved::class,
        'deleted' => UserDeleted::class,
    ];
}
```

#### 任务级的重试&超时

5.4版本之前，队列任务的“重试”和“超时”设置只能在全局的队列配置中指定，在Laravel 5.4中，可以在、通过在任务类中为每一个任务配置独立的“重试”次数和“超时”时间：

```
<?php

namespace App\Jobs;

class ProcessPodcast implements ShouldQueue
{
    /**
     * The number of times the job may be attempted.
     *
     * @var int
     */
    public $tries = 5;

    /**
     * The number of seconds the job can run before timing out.
     *
     * @var int
     */
    public $timeout = 120;
}
```

#### 请求清理中间件

Laravel 5.4在默认中间件堆栈中引入了两个新的中间件：`TrimStrings`和`ConvertEmptyStringsToNull`：

```
/**
 * The application's global HTTP middleware stack.
 *
 * These middleware are run during every request to your application.
 *
 * @var array
 */
protected $middleware = [
    \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
    \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
    \App\Http\Middleware\TrimStrings::class,
    \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
];

```

这两个中间件的作用分别是去除请求输入值首尾两端的空格、将空字符串转化为`null`。这可以帮助我们在每次对应用进行请求的时候对输入值进行归一，而不必在每个路由、每个控制器中重复调用`trim`方法。

#### “实时”门面

#### 自定义透视表模型

#### 优化Redis集群支持

#### 迁移默认字符换长度




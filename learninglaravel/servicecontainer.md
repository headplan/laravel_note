# Service\_Container

服务容器绑定示例

```
$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);

$app->bind(App\ServiceTest\GeneralService::class, function($app){
    return new App\ServiceTest\GeneralService();
});
$app->singleton(App\ServiceTest\SingleService::class, function($app){
    return new App\ServiceTest\SingleService();
});
$instance = new App\ServiceTest\InstanceService();
$app->instance('instanceService', $instance);

$app->bind(
    App\ServiceTest\ServiceContract::class,
    App\ServiceTest\ServiceGeneral::class
);

dd($app);
```

**其他内容查看相关commit**

**更多描述查看核心概念和源码分析中的服务容器相关资料**


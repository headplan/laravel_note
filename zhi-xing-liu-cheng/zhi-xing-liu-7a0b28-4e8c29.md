# 执行流程\(二\)

前面我们已经经历了自动加载 , 到载入应用和初始化应用 , 初始化时我们对定义的服务提供者没有过多的描述 , 这里继续追代码

```php
$this->register(new EventServiceProvider($this));
```

一切从这里开始 , 我们看一下这个服务提供者 , 这也是Laravel第一个服务提供者

```php
class EventServiceProvider extends ServiceProvider
{
    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        $this->app->singleton('events', function ($app) {
            return (new Dispatcher($app))->setQueueResolver(function () use ($app) {
                return $app->make(QueueFactoryContract::class);
            });
        });
    }
}
```




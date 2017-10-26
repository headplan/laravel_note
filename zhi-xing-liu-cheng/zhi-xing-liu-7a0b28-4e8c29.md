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

register方法 , 是在注册时被调用的方法\(还有boot方法\) , 首先就是`$this->app`从哪里来 ? 当然从这里来

```php
new EventServiceProvider($this)
```

这里传递了应用类到服务提供者 , 其实是其抽象父类中已经初始化赋值给了成员属性`$app`

```php
public function __construct($app)
{
    $this->app = $app;
}
```

所以其调用的singleton方法就是应用或者说是其父类Container中的方法 , singleton方法

```php
public function singleton($abstract, $concrete = null)
{
    $this->bind($abstract, $concrete, true);
}
```

这里的参数分别是一个字符串\(events\) , 一个闭包方法 , 闭包方法传入$app , 当然这个就是Application实例 , bind方法的第三个参数是个状态表示共享 , 现在我们看看Container中的这个bind方法

```php
public function bind($abstract, $concrete = null, $shared = false)
{
    // If no concrete type was given, we will simply set the concrete type to the
    // abstract type. After that, the concrete type to be registered as shared
    // without being forced to state their classes in both of the parameters.
    $this->dropStaleInstances($abstract);

    if (is_null($concrete)) {
        $concrete = $abstract;
    }

    // If the factory is not a Closure, it means it is just a class name which is
    // bound into this container to the abstract type and we will just wrap it
    // up inside its own Closure to give us more convenience when extending.
    if (! $concrete instanceof Closure) {
        $concrete = $this->getClosure($abstract, $concrete);
    }

    $this->bindings[$abstract] = compact('concrete', 'shared');

    // If the abstract type was already resolved in this container we'll fire the
    // rebound listener so that any objects which have already gotten resolved
    // can have their copy of the object updated via the listener callbacks.
    if ($this->resolved($abstract)) {
        $this->rebound($abstract);
    }
}
```

绑定之前 , 首先删除就的实例`dropStaleInstances()`

```php
$this->dropStaleInstances($abstract);

protected function dropStaleInstances($abstract)
{
    unset($this->instances[$abstract], $this->aliases[$abstract]);
}
```

然后判断如果闭包是null , 则把抽象赋值给闭包

```php
if (is_null($concrete)) {
    $concrete = $abstract;
}
```




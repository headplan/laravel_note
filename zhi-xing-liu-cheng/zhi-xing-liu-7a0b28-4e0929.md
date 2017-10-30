# 执行流程\(三\)

前面我们经历了注册服务提供者到容器中 , 以初始化注册的第一个服务提供者events为例 , 经历了singleton单例绑定 , 她是bind的一个封装 , 只是默认为shared单例的形式 . 绑定的过程还有几个选择的方式 , 最后更新了bindings这个容器的成员属性 . 最后如果注册的服务提供者没有解析 , 则反弹 . 现在我们来看看前面遗留的几个方法的具体内容是什么 .

首先是getClosure中的build方法 , 她其实也是Container中的方法

```php
public function build($concrete)
{
    // If the concrete type is actually a Closure, we will just execute it and
    // hand back the results of the functions, which allows functions to be
    // used as resolvers for more fine-tuned resolution of these objects.
    if ($concrete instanceof Closure) {
        return $concrete($this, $this->getLastParameterOverride());
    }

    $reflector = new ReflectionClass($concrete);

    // If the type is not instantiable, the developer is attempting to resolve
    // an abstract type such as an Interface of Abstract Class and there is
    // no binding registered for the abstractions so we need to bail out.
    if (! $reflector->isInstantiable()) {
        return $this->notInstantiable($concrete);
    }

    $this->buildStack[] = $concrete;

    $constructor = $reflector->getConstructor();

    // If there are no constructors, that means there are no dependencies then
    // we can just resolve the instances of the objects right away, without
    // resolving any other types or dependencies out of these containers.
    if (is_null($constructor)) {
        array_pop($this->buildStack);

        return new $concrete;
    }

    $dependencies = $constructor->getParameters();

    // Once we have all the constructor's parameters we can create each of the
    // dependency instances and then use the reflection instances to make a
    // new instance of this class, injecting the created dependencies in.
    $instances = $this->resolveDependencies(
        $dependencies
    );

    array_pop($this->buildStack);

    return $reflector->newInstanceArgs($instances);
}
```

首先判断具体类型是不是闭包 , 如果是闭包的话 , 直接执行并


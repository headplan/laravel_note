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

首先判断具体类型是不是闭包 , 如果是闭包的话 , 直接执行这个闭包 . 如果不是则继续执行下面的反射相关的内容 , 这里简单的看一下手册中的内容 :

> [http://php.net/manual/zh/class.reflectionclass.php](http://php.net/manual/zh/class.reflectionclass.php)

这里的ReflectionClass类实现了Reflection接口 , PHP里针对不同的应用常见还实现了其他几个类 , 例如ReflectionMethod,ReflectionFunction等 . 根据手册 , 我们可以继续看build中的代码了 .

```php
if (! $reflector->isInstantiable()) {
    return $this->notInstantiable($concrete);
}

$this->buildStack[] = $concrete;

$constructor = $reflector->getConstructor();
```

首先判断 , 如果类不能实例化 , 则抛出异常 . 否则继续 , 将$concrete具体类型更新到容器的buildStack成员属性中 . 然后获取初始化 .

```php
if (is_null($constructor)) {
    array_pop($this->buildStack);

    return new $concrete;
}
```

如果没有构造函数 , 那就意味着没有依赖项 , 那么我们就可以立即解析对象的实例 , 而不需要从这些容器中解析任何其他类型或依赖项 .

```php
$instances = $this->resolveDependencies(
    $dependencies
);

array_pop($this->buildStack);

return $reflector->newInstanceArgs($instances);
```

如果构造函数有参数 , getConstructor获取构造函数的参数 , 然后使用反射实例来创建这个类的新实例，注入所创建的依赖项 .

再来看看makeWith方法 , 她是resolve的封装 , 同样make方法也是 , 只是差了个默认参数 . 这里我们应该看一下resolve方法

```php
protected function resolve($abstract, $parameters = [])
{
    $abstract = $this->getAlias($abstract);

    $needsContextualBuild = ! empty($parameters) || ! is_null(
        $this->getContextualConcrete($abstract)
    );

    // If an instance of the type is currently being managed as a singleton we'll
    // just return an existing instance instead of instantiating new instances
    // so the developer can keep using the same objects instance every time.
    if (isset($this->instances[$abstract]) && ! $needsContextualBuild) {
        return $this->instances[$abstract];
    }

    $this->with[] = $parameters;

    $concrete = $this->getConcrete($abstract);

    // We're ready to instantiate an instance of the concrete type registered for
    // the binding. This will instantiate the types, as well as resolve any of
    // its "nested" dependencies recursively until all have gotten resolved.
    if ($this->isBuildable($concrete, $abstract)) {
        $object = $this->build($concrete);
    } else {
        $object = $this->make($concrete);
    }

    // If we defined any extenders for this type, we'll need to spin through them
    // and apply them to the object being built. This allows for the extension
    // of services, such as changing configuration or decorating the object.
    foreach ($this->getExtenders($abstract) as $extender) {
        $object = $extender($object, $this);
    }

    // If the requested type is registered as a singleton we'll want to cache off
    // the instances in "memory" so we can return it later without creating an
    // entirely new instance of an object on each subsequent request for it.
    if ($this->isShared($abstract) && ! $needsContextualBuild) {
        $this->instances[$abstract] = $object;
    }

    $this->fireResolvingCallbacks($abstract, $object);

    // Before returning, we will also set the resolved flag to "true" and pop off
    // the parameter overrides for this build. After those two things are done
    // we will be ready to return back the fully constructed class instance.
    $this->resolved[$abstract] = true;

    array_pop($this->with);

    return $object;
}
```

先是获取正确的别名`getAlias`, 然后是判断是不是单例 , 如果配置的是单例则直接返回 , 如果不是 , 我们准备实例化为绑定注册的具体类型的实例 . 这将实例化这些类型, 并以递归方式解析它的任何 "嵌套" 依赖项, 直到全部都得到解决 .

```php
foreach ($this->getExtenders($abstract) as $extender) {
    $object = $extender($object, $this);
}
```

如果我们定义了此类型的任何扩展程序, 我们将需要通过它们进行旋转并将它们应用于正在生成的对象 . 这允许扩展服务, 如更改配置或装饰对象 .

```php
if ($this->isShared($abstract) && ! $needsContextualBuild) {
    $this->instances[$abstract] = $object;
}
```

如果请求的类型被注册为单例, 我们将希望缓存 "内存" 中的实例 , 这样我们以后就可以返回它 , 而不必在其后的每个请求中创建一个完全新的对象实例 .  

然后是触发所有解析回调 : 

```php
$this->fireResolvingCallbacks($abstract, $object);
```

最后更新成员属性$this-&gt;resolved标注意为已经解析过的 . 最后返回object . 


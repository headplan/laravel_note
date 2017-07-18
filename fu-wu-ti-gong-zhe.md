# 服务提供者

为了不让所有功能模块的服务都是在bootstarp/app.php中绑定 , Laravel通过服务提供者解决服务绑定问题 , 每个模块都有一个服务提供者 , 其继承框架提供的Illuminate/Support/ServiceProvider抽象类 , 其中提供了一个虚函数register\(\) , 用于服务绑定 . 程序在启动过程中 , 会遍历所有服务提供者 , 从而完成绑定 .

> 文档中有一句话 , 可以更好的理解容器的绑定 .
>
> 如果一个类没有基于任何接口那么就没有必要将其绑定到容器 . 容器并不需要被告知如何构建对象 , 因为它会使用 PHP 的反射服务自动解析出具体的对象 .

**创建服务提供者**

```
php artisan make:provider TestServiceProvider
```

生成服务提供者 , 当然也可以将生成的文件放在对应的功能模块的目录下 , 改变命名空间完成自动加载即可 . 修改之前服务容器的代码 .

**代码示例查看 , Learning\_Laravel中Service\_Providers分支\(commit -m"服务提供者"\)**

IoC 是将内部设计的类交给系统去控制 , 但是有些类在初始化的时候 , 需要制定特定的参数 , 或者当你需要将实现类绑定到某个接口 , 这时候就必须对这些依赖进行配置 , 系统才能正确解析并引用 .

**register**

而 register 就是这样一个地方 , 你可以在 register 配置类的依赖 , 绑定实现类到接口 , 设置类的别名等等 .

**boot**

而 boot 方法在 register 方法之后调用 , 这就意味着 , 你无须担心在注入某个实例的时候 , 他还没有被绑定或实例化 .

**providers**

用于延迟加载的 ServiceProvider , 比如希望在引用的时候再让系统去解析那个类 , 可以设置 $defer 变量为 true 来延迟启动 , 节省开销

```
protected $defer = true;
```

设置了延迟启动 , 需要重写 providers 函数 .

```
public function providers() {
    return [Test::class];
}
```

最后别忘记注册这个提供者到config/app.php文件的providers数组 , 因为程序运行过程中会调用该类中的registe\(\)方法完成服务绑定 , 所以需要需要用注册的方式来告诉框架新创建的服务提供者类在哪 . 

> 这里都是用了php5.5获取类名的方法::class




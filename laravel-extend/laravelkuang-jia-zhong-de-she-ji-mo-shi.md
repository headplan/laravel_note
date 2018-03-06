# Laravel框架中的设计模式

资料参考:

&lt;书&gt;

[https://www.insp.top/article/learn-laravel-container](https://www.insp.top/article/learn-laravel-container)

[http://www.digpage.com/di.html](http://www.digpage.com/di.html)

[https://martinfowler.com/articles/injection.html](https://martinfowler.com/articles/injection.html)

### 服务容器

服务容器是变量,属性,对象,文件路径,系统配置等这些容器的载体,在系统运行过程中动态的为系统提供这些服务.

载体中的容器如果是对象,对象的描述\(类,接口\)或者是提供对象的回调,就可以实现很多高级的功能,例如"解耦","依赖注入\(DI\)".

#### 依赖与耦合

**解决依赖,实现解耦才是应用服务容器最关键的原因.**

**几个概念:**

> **依赖倒置原则**\(Dependence Inversion Principle, DIP\)
>
> DIP是一种软件设计的指导思想,核心思想是上层定义接口,下层实现这个接口.改变了原有的上下依赖,倒置为下上依赖.降低耦合度,提高整个系统的弹性.
>
> **控制反转\(**Inversion of Control, IoC\)
>
> IoC就是DIP的一种具体思路,DIP只是一种理念、思想,而IoC是一种实现DIP的方法.
>
> IoC的核心是将类\(上层\)所依赖的单元\(下层\)的实例化过程交由第三方来实现.
>
> 简单的解释就是类中不new依赖的实例.
>
> **依赖注入**\(Dependence Injection, DI\)
>
> DI是IoC的一种设计模式,是一种套路,按照DI的套路,就可以实现IoC,就能符合DIP原则.DI的核心是把类所依赖的单元的实例化过程,放到类的外面去实现.
>
> **控制反转容器**\(IoC Container\)
>
> IoC Container提供了动态地创建、注入依赖单元,映射依赖关系等功能,减少了许多代码量.
>
> **服务定位器**\(Service Locator\)
>
> 核心是把所有可能用到的依赖单元交由Service Locator进行实例化和创建、配置， 把类对依赖单元的依赖,转换成类对Service Locator的依赖.过IoC容器,实现了Service Locator.

```
 # 实现依赖注入
 # 例子:要实现当访客在博客上发表评论后,向博文的作者发送Email的功能.这里的邮件发送服务有多种.
```

依赖注入的注入方式,可以是构造函数注入,也可以是属性注入.现在又遇到一个问题,依赖单元的实例化是一个重复,繁琐的过程.这里又引入一个模式,工厂模式,来管理代码中的邮件服务.






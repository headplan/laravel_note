# 目录结构

Laravel 应用默认的目录结构试图为不管是大型应用还是小型应用提供一个好的起点，当然，你可以自己按照喜好重新组织应用目录结构，Laravel 对类在何处被加载没有任何限制——只要 Composer 可以自动载入它们即可。

> **Models消歧义**
>
> 许多初学者都会困惑 Laravel 为什么没有`models`目录，我可以负责任的告诉大家，这是故意的。因为`models`这个词对不同人而言有不同的含义，容易造成歧义，有些开发者认为应用的模型指的是业务逻辑，另外一些人则认为模型指的是与关联数据库的交互。

### App目录 {#toc_2}

应用的核心代码位于`app`目录下，默认情况下，该目录位于命名空间`App`下， 并且被 Composer 通过[PSR-4自动载入标准](http://www.php-fig.org/psr/psr-4/)自动加载。

`app`目录下包含多个子目录，如`Console`、`Http`、`Providers`等。`Console`和`Http`目录提供了进入应用核心的API，HTTP协议和CLI是和应用进行交互的两种机制，但实际上并不包含应用逻辑。换句话说，它们只是两个向应用发布命令的方式。`Console`目录包含了所有的Artisan命令，`Http`目录包含了控制器、中间件和请求等。

其他目录会在你通过 Artisan 命令`make`生成相应类的时候生成到`app`目录下。例如，`app/Jobs`目录直到你执行`make:job`命令生成任务类时才会出现在`app`目录下。

> 注意：`app`目录中的很多类都可以通过 Artisan 命令生成，要查看所有有效的命令，可以在终端中运行`php artisan list make`命令。

**Console目录**

`Console`目录包含应用所有自定义的 Artisan 命令，这些命令类可以使用`make:command`命令生成。该目录下还有 Console Kernel 类，在这里可以注册自定义的 Artisan 命令以及定义[调度任务](https://laravel.com/docs/5.4/scheduling)。

**Events目录**

这个目录默认不存在，但是可以通过`event:generate`和`make:event`命令创建。该目录用于存放[事件类](https://laravel.com/docs/5.4/events)。事件类用于告知应用其他部分某个事件发生并提供灵活的、解耦的处理机制。

**Exceptions目录**

`Exceptions`目录包含应用的异常处理器，同时还是处理应用抛出的任何异常的好地方。如果你想要自定义异常如何记录异常或渲染，需要修改`Handler`类。

**Http目录**

`Http`目录包含了控制器、中间件以及表单请求等，几乎所有进入应用的请求处理都在这里进行。

**Jobs目录**

该目录默认不存在，可以通过执行`make:job`命令生成，`Jobs`目录用于存放[队列任务](https://laravel.com/docs/5.4/queues)，应用中的任务可以被推送到队列，也可以在当前请求生命周期内同步执行。同步执行的任务有时也被看作命令，因为它们实现了[命令模式](http://laravelacademy.org/post/2871.html)。

**Listeners目录**

这个目录默认不存在，可以通过执行`event:generate`和`make:listener`命令创建。`Listeners`目录包含处理[事件](https://laravel.com/docs/5.4/events)的类（事件监听器），事件监听器接收一个事件并提供对该事件发生后的响应逻辑，例如，`UserRegistered`事件可以被`SendWelcomeEmail`监听器处理。

**Mail目录**

这个目录默认不存在，但是可以通过执行`make:mail`命令生成，`Mail`目录包含邮件发送类，邮件对象允许你在一个地方封装构建邮件所需的所有业务逻辑，然后使用`Mail::send`方法发送邮件。

**Notifications目录**

这个目录默认不存在，你可以通过执行`make:notification`命令创建，`Notifications`目录包含应用发送的所有通知，比如事件发生通知。Laravel 的通知功能将通知发送和通知驱动解耦，你可以通过邮件，也可以通过Slack、短信或者数据库发送通知。

**Policies目录**

这个目录默认不存在，你可以通过执行`make:policy`命令来创建，`Policies`目录包含了所有的授权策略类，策略用于判断某个用户是否有权限去访问指定资源。更多详情，请查看[授权文档](https://laravel.com/docs/5.4/authorization)。

**Providers目录**

`Providers`目录包含应用的所有[服务提供者](https://laravel.com/docs/5.4/providers)。服务提供者在启动应用过程中绑定服务到容器、注册事件以及执行其他任务以为即将到来的请求处理做准备。

在新安装的 Laravel 应用中，该目录已经包含了一些服务提供者，你可以按需添加自己的服务提供者到该目录。




# 目录结构

Laravel 应用默认的目录结构试图为不管是大型应用还是小型应用提供一个好的起点，当然，你可以自己按照喜好重新组织应用目录结构，Laravel 对类在何处被加载没有任何限制——只要 Composer 可以自动载入它们即可。

> **Models消歧义**
>
> 许多初学者都会困惑 Laravel 为什么没有`models`目录，我可以负责任的告诉大家，这是故意的。因为`models`这个词对不同人而言有不同的含义，容易造成歧义，有些开发者认为应用的模型指的是业务逻辑，另外一些人则认为模型指的是与关联数据库的交互。

### 根目录 {#toc_1}

**App目录**

`app`目录包含了应用的核心代码，此外你为应用编写的代码绝大多数也会放到这里；

**Bootstrap目录**

`bootstrap`目录包含了少许文件，用于框架的启动和自动载入配置，还有一个`cache`文件夹用于包含框架为提升性能所生成的文件，如路由和服务缓存文件；

**Config目录**

`config`目录包含了应用所有的配置文件，建议通读一遍这些配置文件以便熟悉所有配置项；

**Database目录**

`database`目录包含了数据迁移及填充文件，如果你喜欢的话还可以将其作为 SQLite 数据库存放目录；

**Public目录**

`public`目录包含了入口文件`index.php`和前端资源文件（图片、JavaScript、CSS等）；

**Resources目录**

`resources`目录包含了视图文件及原生资源文件（LESS、SASS、CoffeeScript），以及本地化语言文件；

**Routes目录**

`routes`目录包含了应用的所有路由定义。Laravel默认提供了三个路由文件：`web.php`、`api.php`和`console.php`。

`web.php`文件包含的路由都会应用web中间件组，具备Session、CSRF防护以及Cookie加密功能，如果应用无需提供无状态的、RESTful风格的API，所有路由都会定义在`web.php`文件。

`api.php`文件包含的路由应用了`api`中间件组，具备频率限制功能，这些路由是无状态的，所以请求通过这些路由进入应用需要通过token进行认证并且不能访问Session状态。

`console.php`文件用于定义所有基于闭包的控制台命令，每个闭包都被绑定到一个控制台命令并且允许与命令行IO方法进行交互，尽管这个文件并不定义HTTP路由，但是它定义了基于控制台的应用入口（路由）。

**Storage目录**

`storage`目录包含了编译过的Blade模板、基于文件的session、文件缓存，以及其它由框架生成的文件，该目录被细分为成`app`、`framework`和`logs`子目录，`app`目录用于存放应用要使用的文件，`framework`目录用于存放框架生成的文件和缓存，最后，`logs`目录包含应用的日志文件；

`storage/app/public`目录用于存储用户生成的文件，比如可以被公开访问的用户头像，要达到被访问的目的，你还需要在`public`目录下生成一个软连接`storage`指向这个目录。你可以通过`php artisan storage:link`命令生成这个软链接。

**Tests目录**

`tests`目录包含自动化测试，其中已经提供了一个开箱即用的PHPUnit示例；每一个测试类都要以`Test`开头，你可以通过`phpunit`或`php vendor/bin/phpunit`命令来运行测试。

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


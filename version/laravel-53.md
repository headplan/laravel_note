# Laravel 5.3

## 目录结构

### **简介**

默认的目录结构试图为不管大型应用还是小型应用提供一个好的起点,可以按照自己的喜好重新组织应用目录结构,Laravel对类在何处被加载没有任何限制,只要Composer可以自动载入即可.

**Models目录**

Laravel没有Models目录,因为models容易造成歧义,有的人认为模型指的是业务逻辑,有的则认为模型指的是关联数据库的交互.所以,默认将Eloquent的模型直接放置到App目录下,从而允许开发者自行选择放置的位置.

### 根目录

* **App目录**:包含了应用的核心代码,另外编写应用的绝大部分代码都会放在这里.
* **Bootstrap目录**:包含了少许文件,用于框架的启动和自动载入配置,还有一个cache文件夹用于包含框架为提升性能所生成的文件,如路由和服务缓存文件.
* **Config目录**:包含了应用所有的配置文件.
* **Database目录**:包含了数据迁移及填充文件,如果喜欢的话还可以将其作为SQLite数据库存放目录.
* **Public目录**:包含了入口文件index.php和前端资源文件\(图片,JavaScript,CSS等\).
* **Resources目录**:包含了视图文件及原生资源文件\(LESS,SASS,CoffeeScript\),以及本地化文件.
* **Routes目录**:包含了应用的所有路由定义.默认提供了三个路由文件.

  * **web.php** - 文件包含的路由都会应用web中间件组,具备Session,CSRF防护以及Cookie加密功能,如果应用无需提供无状态的,RESTful风格的API,所有路由都会定义在web.php文件.
  * **api.php** - 文件包含的路由应用了api中间件组,具备频率限制功能,这些路由是无状态的,所以请求通过这些路由进入应用需要通过token进行认证并且不能访问Session状态.
  * **console.php** - 文件用于定义所有基于闭包的控制台命令,每个闭包都被绑定到一个控制台命令并且允许与命令行IO方法进行交互,尽管这个文件并不定义HTTP路由,但是它定义了基于控制台的应用入口\(路由\).

* **Storage目录**:包含了编译过的Blade模板,基于文件的session,文件缓存,以及其它由框架生成的文件.

  * **app目录** - 用于存放应用要使用的文件.

    * **public目录** - 用于存储用户生成的文件,比如可以被公开访问的用户头像.要达到被访问的目的,需要在`public`目录下生成一个软连接 `storage` 指向这个目录.可以通过 `php artisan storage:link` 命令生成这个软链接.

  * **framework目录** - 用于存放框架生成的文件和缓存.

  * **logs目录** - 目录包含应用的日志文件.



* **Tests目录**:包含自动化测试,其中已经提供了一个开箱即用的PHPUnit示例.每一个测试类都要以 Test 开头,可以通过 `phpunit` 或 `php vendor/bin/phpunit` 命令来运行测试.

* **Vendor目录**:包含了Composer依赖.


### App目录说明

应用的核心代码位于app目录下,默认位于命名空间App下,并且被PSR-4自动加载.

app目录下包含多个子目录,其中Console和Http目录提供了进入应用核心的API,HTTP协议和CLI是和应用进行交互的两种机制,但实际上并不包含应用逻辑,换句话说,它们只是两个向应用发布命令的方式.Console包含了所有的Artisan命令,Http目录包含了控制器,中间件和请求等.

其他目录将会在你通过Artisan命令make生成相应类的时候生成到`app`目录下.`app`目录中的很多类都可以通过Artisan命令生成,要查看所有有效的命令,可以在终端中运行`php artisan list make`命令.

* **Console目录**:包含应用所有自定义的Artisan命令,这些命令类可以使用`make:command`命令生成.该目录下还有console核心类,在这里可以注册自定义的Artisan命令以及定义调度任务.
* **Exceptions目录**:包含应用的异常处理器,同时还是处理应用抛出的任何异常的好地方.如果你想要自定义异常如何记录异常或渲染,需要修改Handler类.
* **Http目录**:包含了控制器、中间件以及表单请求等,几乎所有进入应用的请求处理都在这里进行.
* **Providers目录**:包含应用的所有服务提供者.服务提供者在启动应用过程中绑定服务到容器、注册事件以及执行其他任务以为即将到来的请求处理做准备.
* 默认不存在的目录

  * **Events目录** - 可以通过 `event:generate` 和 `event:make` 命令创建.该目录用于存放事件类.事件类用于告知应用其他部分某个事件发生并提供灵活的、解耦的处理机制.
  * **Jobs目录** - 可以通过执行 `make:job` 命令生成,`Jobs`目录用于存放队列任务,应用中的任务可以被队列化,也可以在当前请求生命周期内同步执行.同步执行的任务有时也被看作命令,因为它们实现了命令模式.
  * **Listeners目录** - 可以通过执行 `event:generate` 和 `make:listener` 命令创建.`Listeners`目录包含处理事件的类\(事件监听器\),事件监听器接收一个事件并提供对该事件发生后的响应逻辑.例如,`UserRegistered`事件可以被`SendWelcomeEmail`监听器处理.
  * **Mail目录** - 可以通过执行 `make:mail` 命令生成,`Mail`目录包含邮件发送类,邮件对象允许你在一个地方封装构建邮件所需的所有业务逻辑,然后使用 `Mail::send` 方法发送邮件.

  * **Notifications目录** - 可以通过执行 `make:notification` 命令创建,`Notifications` 目录包含应用发送的所有通知,比如事件发生通知.Laravel的通知功能将通知发送和通知驱动解耦,你可以通过邮件,也可以通过Slack、短信或者数据库发送通知.

  * **Policies目录** - 可以通过执行 `make:policy` 命令来创建,`Policies`目录包含了所有的授权策略类,策略用于判断某个用户是否有权限去访问指定资源.




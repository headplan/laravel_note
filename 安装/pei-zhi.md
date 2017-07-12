# 配置

Laravel 的所有配置文件都存放在 config 目录下，每个配置项都有注释，以保证浏览任意配置文件的配置项都能直观了解该配置项的作用。

### 环境配置

基于应用运行的环境不同设置不同的配置值能够给我们开发带来极大的方便，比如，我们通常在本地和线上环境配置不同的缓存驱动，这一机制在 Laravel 中很容易实现。

Laravel 使用 Vance Lucas 开发的 PHP 库[DotEnv](https://github.com/vlucas/phpdotenv)来实现这一机制，在新安装的 Laravel 中，根目录下有一个`.env.example`文件，如果 Laravel 是通过 Composer 安装的，那么该文件已经被重命名为`.env`，否则的话你要自己手动重命名该文件。

> 还可以创建一个`.env.testing`文件，该文件会在运行 PHPUnit 测试或执行带有`--env=testing`选项的 Artisan 命令时覆盖从`.env`文件读取的值。

#### **获取环境变量配置值**

在应用每次接受请求时，`.env`中列出的所有配置及其值都会被载入到 PHP 超全局变量`$_ENV`中，然后你就可以在应用中通过辅助函数`env`来获取这些配置值。实际上，如果你去查看 Laravel 的配置文件，就会发现很多地方已经在使用这个辅助函数了：

```
'debug' => env('APP_DEBUG', false),
```

传递到`env`函数的第二个参数是默认值，如果环境变量没有被配置将会是个该默认值。

不要把`.env`文件提交到源码控制（svn 或 git 等）中，因为每个使用你的应用的开发者/服务器可能要求不同的环境配置。

如果你是在一个团队中进行开发，你需要将`.env.example`文件随你的应用一起提交到源码控制中：将一些配置值以占位符的方式放置在`.env.example`文件中，这样其他开发者就会很清楚运行你的应用需要配置哪些环境变量。

#### 判断当前应用环境 {#toc_2}

当前应用环境由`.env`文件中的`APP_ENV`变量决定，你可以通过`App`门面的`environment`方法来访问其值：

```
$environment = App::environment();
```

你也可以向`environment`方法中传递参数来判断当前环境是否匹配给定值，如果需要的话你甚至可以传递多个值。如果当前环境与给定值匹配，该方法返回`true`：

```
if (App::environment('local')) {
    // The environment is local
}

if (App::environment('local', 'staging')) {
    // The environment is either local OR staging...
}
```

#### 访问配置值

你可以使用全局辅助函数`config`在应用的任意位置访问配置值，该配置值可以文件名+“.”+配置项的方式进行访问，当配置项没有被配置的时候返回默认值：

```
$value = config('app.timezone');
```

如果要在运行时设置配置值，传递数组参数到`config`方法即可：

```
config(['app.timezone' => 'America/Chicago']);
```

#### 配置缓存

为了给应用加速，你可以使用 Artisan 命令`config:cache`将所有配置文件的配置缓存到单个文件里，这将会将所有配置选项合并到单个文件从而可以被框架快速加载。

应用一旦上线，就要运行一次`php artisan config:cache`，但是在本地开发时，没必要经常运行该命令，因为配置值经常需要改变。

> 如果在部署过程中执行`config:cache`命令，确保只在配置文件中调用了`env`方法。

#### 维护模式

当你的应用处于维护模式时，所有对应用的请求都会返回同一个自定义视图。这一机制在对应用进行升级或者维护时，使得“关闭”站点变得轻而易举。对维护模式的判断代码位于应用默认的中间件栈中，如果应用处于维护模式，则状态码为`503`的`MaintenanceModeException`将会被抛出。

要开启维护模式，只需执行 Artisan 命令`down`即可：

```
php artisan down
```

还可以提供`message`和`retry`选项给`down`命令。`message`的值用于显示和记录自定义消息，而`retry`的值用于设置HTTP请求头的`Retry-After`：

```
php artisan down --message="Upgrading Database" --retry=60
```

要关闭维护模式，对应的 Artisan 命令是`up`：

```
php artisan up
```

**维护模式响应模板**

默认的维护模式响应视图模板是`resources/views/errors/503.blade.php`，如果需要的话你可以修改这个视图文件。

**维护模式 & 队列**

当你的站点处于维护模式中时，所有的[队列任务](https://laravel.com/docs/5.4/queues)都不会执行；当应用退出维护模式这些任务才会被继续正常处理。

**维护模式的替代方案**

由于维护模式命令的执行需要几秒时间，你可以考虑使用[Envoyer](https://envoyer.io/)实现 0 秒下线作为替代方案。


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

  
  





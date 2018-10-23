# Xdebug

PHP Note中有详细的安装配置以及参数解释函数等记录 , 这里仅记录快速安装配置Laravel相关的Xdebug .

#### **原理回顾**

* IDE一般都会集成了一个遵循BGDp的Xdebug插件 . 当要debug的时候 , 点击一些启动插件即可 . 插件会启动一个默认9000的端口监听远程服务器发过来的debug信息 . 

> 例如在PHPSTORM中开启 , Run&gt;Listening for PHP Xdebug Connetions .

* 浏览器向Http服务器发送一个带有XDEBUG\_SESSION\_START参数的请求 , 服务器收到这个请求之后交给后端的PHP\(已开启 xdebug 模块\)进行处理 . 
* PHP接收XDEBUG\_SESSION\_START请求之后响应给Xdebug , Xdebug请求客户端9000端口 , 然后客户端响应 . 这里Xebug有两种模式通知客户端可以配置 , xdebug.remote\_connect\_back = 0 \| 1 . 
* PHP开始执行代码并让Xdebug过滤 , 然后发给9000端口 . 
* 客户端 , 也就是编辑器展示响应结果 . 

两种配置方式 :

xdebug.remote\_connect\_back = 0 \(默认\)

需要配置`xdebug.remote_host`和`xdebug.remote_port`

交互流程

![](/assets/xdebug_yuanli1.png)

remote\_host的IP是固定的 , 这种方式只适合单一客户端开发调试 .

也可以不绑定IP , xdebug.remote\_connect\_back = 1

不同的是 , php在接受http请求后 , Xdebug会将请求来源的IP绑定 . 并通知 .

#### Xdebug配置

```
zend_extension="/usr/lib/php/20160303/xdebug.so"
xdebug.remote_enable = 1
xdebug.idekey = PHPSTORM
xdebug.remote_connect_back = 1
xdebug.remote_log = "/tmp/xdebug_php71.log"
```

PHP 有两种运行模式`FPM`和`CLI`

开启模块 :

```
sudo phpenmod -s fpm -v 7.1 xdebug
```

这样就会只开启`PHP-FPM`的Xdebug模块 , 而不会影响`CLI`

开启模块后 , 重启一下服务 :

```
sudo service php7.1-fpm restart
```

#### PHPStorm配置

##### 针对单个PHP文件调试配置

设置CLI Interpreter , 这里设置Homestead中的PHP解释器 , 填写配置文件 , 以SSH的方式访问即可 .

如果CLI模式没有开始 , 也可以使用命令开启

```
sudo phpenmod -s cli -v 7.1 xdebug
# 与其相反的命令是
phpdismod
```

在编辑器中添加了这个CLI Interpreter之后 , 就可以选择并配置了 , 下面的Path mapping配置可以用来设置目录的映射 .

**针对整个项目的调试配置**

针对整个项目的配置和前面的没有相关性 , 这里需要配置一个server , 当然目录还是要映射对 .

![](/assets/xdebug_setting.png)

参考文章 :

[https://my.oschina.net/atanl/blog/371424](https://my.oschina.net/atanl/blog/371424)

[https://laravel-china.org/articles/4090/the-first-step-to-becoming-a-senior-php-programmer-debugging-xdebug-principle](https://laravel-china.org/articles/4090/the-first-step-to-becoming-a-senior-php-programmer-debugging-xdebug-principle)

[https://laravel-china.org/articles/4098/the-first-step-to-becoming-a-senior-php-programmer-debug-xdebug-configuration](https://laravel-china.org/articles/4098/the-first-step-to-becoming-a-senior-php-programmer-debug-xdebug-configuration)


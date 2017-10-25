# 执行流程

#### 自动加载

经历过的文件 :

* 从 **public/index.php** 入口定位到 **bootstrap/autoload.php**
* 从 **bootstrap/autoload.php** 定位到 **vendor/autoload.php**
* 从 **vendor/autoload.php** 定位到`__DIR__ . '/composer' . '/autoload_real.php';`

这就是我们要找的文件**autoload\_real.php **, 文件中就是我们要找的 , 调用其方法 :

```
ComposerAutoloaderInit10488597059b401fe5386bdff1f41d9d::getLoader();
```

先从getLoader\(\)开始 .

**getLoader\(\)**

她是一个静态方法

```php
public static function getLoader()
```

第一部分内容 , 判断如果静态变量`$loader`不为空则返回`$loader`

```php
if (null !== self::$loader) {
    return self::$loader;
}
```

然后是注册一个自动加载程序 , 加载程序为本类的`loadClassLoader()`方法 , 先看一下这个方法

```php
public static function loadClassLoader($class)
{
    if ('Composer\Autoload\ClassLoader' === $class) {
        require __DIR__ . '/ClassLoader.php';
    }
}
```

静态方法 , 含有一个`$class`参数 , 判断如果`$class`等于`Composer\Autoload\ClassLoader` , 则载入当前目录下 的 ClassLoader.php 文件 , 实际上是这只是个入口 , 其目的也是为后面赋值用的 .

```php
spl_autoload_register(array('ComposerAutoloaderInit10488597059b401fe5386bdff1f41d9d', 'loadClassLoader'), true, true);
self::$loader = $loader = new \Composer\Autoload\ClassLoader();
spl_autoload_unregister(array('ComposerAutoloaderInit10488597059b401fe5386bdff1f41d9d', 'loadClassLoader'));
```

这里首先注册本类中的`loadClassLoader`函数 , 其参数$class就是后面`new \Composer\Autoload\ClassLoader();`的类名 , 然后按照`loadClassLoader`函数中的规则 , 引入了`ClassLoader.php`文件 , 最后再unregister这个函数`loadClassLoader`.简单来说 , 就是`$loader`得到`ClassLoader`类\(`\Composer\Autoload\ClassLoader`\)的一个实例 , 卸载自动加载程序 loadClassLoader . 

得到了$loader , 然后就是载入设置一些路径信息 , 新版本的composer加入了autoload\_static.php文件 , 也就是在composer 1.1.0 版本开始如果php的版本大于5.6等条件 , composer 会进行加载优化 . 

```php
$useStaticLoader = PHP_VERSION_ID >= 50600 && !defined('HHVM_VERSION') && (!function_exists('zend_loader_file_encoded') || !zend_loader_file_encoded());
if ($useStaticLoader) {
    require_once __DIR__ . '/autoload_static.php';

    call_user_func(\Composer\Autoload\ComposerStaticInit10488597059b401fe5386bdff1f41d9d::getInitializer($loader));
} else {
    $map = require __DIR__ . '/autoload_namespaces.php';
    foreach ($map as $namespace => $path) {
        $loader->set($namespace, $path);
    }

    $map = require __DIR__ . '/autoload_psr4.php';
    foreach ($map as $namespace => $path) {
        $loader->setPsr4($namespace, $path);
    }

    $classMap = require __DIR__ . '/autoload_classmap.php';
    if ($classMap) {
        $loader->addClassMap($classMap);
    }
}
```




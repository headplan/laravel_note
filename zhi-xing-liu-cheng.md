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




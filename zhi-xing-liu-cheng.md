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




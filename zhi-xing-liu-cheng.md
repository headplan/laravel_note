# 执行流程

#### 自动加载

经历过的文件 : 

从 **public/index.php** 入口定位到 **bootstrap/autoload.php**

从 **bootstrap/autoload.php** 定位到 **vendor/autoload.php**

从 **vendor/autoload.php** 定位到**`__DIR__ . '/composer' . '/autoload_real.php';`**




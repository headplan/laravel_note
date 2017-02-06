# 组件化开发与Composer使用

组件化开发的目的就是能够快速使用已有的程序模块构建项目,甚至可以快速更换项目中的相应模块而不需要修改系统中其他部分的代码,这就需要所有的代码按照一定的规范和接口来实现.

> PHP-FIG - PHP Framework Interop Group , 就是定制一系列PHP开发的规范即PSR编码规范标准

不用修改框架中的其他部分代码就能保证各模块间协调工作,这就是规范和接口的威力.

不同的编程语言都有自己的组件化管理工具.

* PHP - Composer
* Ruby - gem
* Java - maven
* Python - pip

Composer需要PHP5.3支持,建议安装PHP5.4以上.这里Composer的安装步骤暂时略过.

### 创建项目

在完成Composer工具的安装后,就可以通过组件化的方式创建项目了.Composer官方提供了组件资源库packagist\(packagist.org\),可以搜索自己想要的资源包.

创建composer.json文件

```
{
    "name": "headplan/codebase", # 自定义的名字
    "require": {                 # 需要的资源包
        "monolog/monolog": "1.*" # 资源包名字和版本约束号
    }
}
```

然后执行composer install命令安装即可.

Composer会检查json文件中的组件名称及版本,然后创建并下载到vendor文件夹中\(vendor/monolog/monolog\).完成下载后创建composer.lock文件锁,记录当前项目的依赖组件和确切的版本号,这点对于分布式开发非常有用,不同的开发人员只需要上传composer.lock文件到版本库,其他人通过lock文件就可以下载相同的组件,实现程序的版本统一.

#### 自动加载

Composer不仅可以下载组件,同时还搭配了自动加载功能,在项目中引入

```
require 'vendor/autoload.php';
```

即可以直接使用下载的组件类.

```
<?php
require 'vendor/autoload.php';

$myLog = new \Monolog\Logger('YuAn');
var_dump($myLog);
```

Composer提供了四种自动加载规范

* PSR-0
* PSR-4
* classmap
* files

四种规范形式上定义了一个命名空间到实际文件的映射关系.在composer.json中可以直接添加autoload字段实现命名空间到目录的映射,例如

```
{
    "autoload": {
        "psr-4": {"App\\": "app/"},
        "psr-0": {"Bpp\\": "bpp/"},
        "classmap": ["database"],
        "files": [
            "src/app/helpers.php",
            "src/bpp/helpers.php"
        ]
    }
}
```

四种规范的解释

* 在psr-4规范下,创建app/User.php文件,该文件需要包含App\User类,实例化\App\User\(\)类时,自动加载app/User.php文件
* 在psr-0规范下,创建bpp/Bpp/User.php文件,该文件需要包含Bpp\User类,才符合自动加载规则,比psr-4多了Bpp一层目录
* 对于classmap会扫描指定目录中所有的.php和.inc文件,并加载到vendor/composer/autoload\_classmap.php文件中,这是一个类和文件映射关系的数组,这里的自动加载可以不符合上面的规则.
* 通过files规范加载一些每次执行时都需要载入的文件,一般函数库文件都会使用这种载入方式.

#### Composer命令行

* composer list - 获取帮助信息
* composer init - 以交互方式填写composer.json文件信息
* composer install - 从当前目录读取composer.json文件,处理依赖关系,并安装到vendor目录下
* composer update - 获取依赖的最新版本,升级composer.lock文件
* composer require - 添加新的依赖包到composer.json文件中并执行更新
* composer search - 在当前项目中搜索依赖包
* composer show - 列举所有可用的资源包
* composer validate - 检测composer.json文件是否有效
* composer self-update - 将composer工具更新到最新版本
* composer create-project - 基于composer创建一个新的项目
* composer dump-autoload - 在添加新的类和目录映射时更新autoloader

### 手动构建Laravel框架




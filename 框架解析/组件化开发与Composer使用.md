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

然后执行composer install命令安装即可


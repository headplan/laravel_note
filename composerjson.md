# composer.json

```
{
    "name": "laravel/laravel", // 项目名称
    "description": "The Laravel Framework.", // 描述
    "keywords": ["framework", "laravel"], // 关键词
    "license": "MIT", // 许可协议
    "type": "project", // 类型
    "require": {
        "php": ">=5.5.9", // PHP版本
        "laravel/framework": "5.2.*" // 框架版本
    },
    "require-dev": { // 依赖包
        "fzaninotto/faker": "~1.4",
        "mockery/mockery": "0.9.*",
        "phpunit/phpunit": "~4.0",
        "symfony/css-selector": "2.8.*|3.0.*",
        "symfony/dom-crawler": "2.8.*|3.0.*"
    },
    "autoload": { // 自动加载
        "classmap": [
            "database"
        ],
        "psr-4": {
            "App\\": "app/"
        }
    },
    "autoload-dev": { // 自动加载
        "classmap": [
            "tests/TestCase.php"
        ]
    },
    "scripts": { // 执行脚本
        "post-root-package-install": [
            "php -r \"copy('.env.example', '.env');\"" // 复制.env.example文件
        ],
        "post-create-project-cmd": [
            "php artisan key:generate" // 生成随机key值
        ],
        "post-install-cmd": [
            "Illuminate\\Foundation\\ComposerScripts::postInstall", // 安装
            "php artisan optimize" // 清除安装过程的缓存或垃圾文件
        ],
        "post-update-cmd": [
            "Illuminate\\Foundation\\ComposerScripts::postUpdate",
            "php artisan optimize"
        ]
    },
    "config": { // 配置项
        "preferred-install": "dist" // 优先安装压缩版
    }
    "repositories": // 可以配置composer镜像
}
```


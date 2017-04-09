# Summerblue-generator

[https://github.com/summerblue/generator](https://github.com/summerblue/generator)

> 一条命令把一个模块需要的基本都生成了.

```
# 功能列表
    # 生成数据库迁移;
    # 数据填充+更新DatabaseSeeder类,添加模型工厂;
    # 基础数据模型Model类和辅助trait;
    # 简洁的RESTful资源控制器,没有注释;
    # 基础表单请求FormRequest类和每个模型对应的StoreRequest,UpdateRequest类;
    # 基础授权策略Policy类和数据模型对应的Policy,并自动注册到AuthServiceProvider类中;
    # 自动注册资源控制器至路由文件中;
    # 增加error视图;
    # create和edit动作使用相同的视图文件;
```

### 安装

```
composer require 'summerblue/generator' --dev
# /app/Providers/AppServiceProvider.php 在 register 方法添加
public function register()
{
     if (app()->environment() == 'local' || app()->environment() == 'testing') {

        $this->app->register(\Summerblue\Generator\GeneratorsServiceProvider::class);

    }
}
```

查看命令

```
# php artisan 运行命令之后可以查看到这个命令
php artisan make:scaffold
```

### 命令示例

```
php artisan make:scaffold Projects --schema="name:string:index,description:text:nullable,subscriber_count:integer:unsigned,default(0)"
```




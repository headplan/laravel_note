# 实例开发

**新建项目**

```
composer create-project laravel/laravel ./Lartisan-API "5.4.*" --prefer-dist
```

简单配置一下.env文件

**创建数据表**

```
php artisan make:migration create_article_table --create=articles
php artisan make:model Article
php artisan make:controller ArticlesController
```

创建数据库

**配置migration文件**

简单设置几个字段

```php
public function up()
{
    Schema::create('articles', function (Blueprint $table) {
        $table->increments('id');
        $table->string('title');
        $table->string('subtitle');
        $table->text('content');
        $table->boolean('status');
        $table->timestamps();
    });
}
```

生成数据表 : `php artisan migrate`




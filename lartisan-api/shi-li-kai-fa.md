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




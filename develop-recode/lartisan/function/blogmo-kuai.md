# 专题模块

生成数据迁移以及基本内容

```
php artisan make:migration create_article_categories_table
php artisan make:migration create_article_topics_table
php artisan migrate:fresh --seed
```

```
# 添加路由,创建模型,创建路由器
php artisan make:model Models/Articles 指定table
php artisan make:controller ArticlesController --resource

# 修改头部入口链接,没有专栏默认进入创建专栏页,有则进入添加内容页.
专栏路由:articles/columns
# 基本的完成,暂时备份,整理list完善流程.
```

---




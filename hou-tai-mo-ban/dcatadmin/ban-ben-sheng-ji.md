# 版本升级

只有像从`v1.0.0`到`v2.0.0`这样的大版本升级才可能会有兼容性问题 , 小版本则是完全兼容的 . 

#### 升级命令

```
composer update dcat/laravel-admin
```

升级成功后一般需要重新发布语言包、配置文件、前端静态资源等文件 , 然后清理浏览器缓存

```
php artisan admin:publish --force
```

**单独更新**

```
# 只更新语言包
php artisan admin:publish --force --lang

# 只更新配置文件
php artisan admin:publish --force --config

# 只更新前端静态资源
php artisan admin:publish --force --assets

# 只更新数据库迁移文件(这个一般不需要更新)
php artisan admin:publish --force --migrations
```




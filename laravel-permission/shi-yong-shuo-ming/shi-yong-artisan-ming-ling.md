# 使用artisan命令

创建角色或权限 : 

```
php artisan permission:create-role writer
php artisan permission:create-permission 'edit articles'
```

指定Guard创建 : 

```
php artisan permission:create-role writer web
php artisan permission:create-permission 'edit articles' admin
```




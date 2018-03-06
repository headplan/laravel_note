# 数据初始化

**创建分支**

```
git checkout master
git checkout -b database
```

#### 数据库迁移

在database/migrations中 , 已经准备了两个默认迁移文件 . 迁移文件中有两个方法 :

* up\(\)方法运行迁移,创建Schema::create\(\)表
* down\(\)方法回滚迁移,删除Schema::dropIfExists\(\)表

#### 常用迁移命令

```
php artisan migrate # 运行数据库迁移
migrate:fresh       # 删除所有表并重新运行所有迁移,物理删除数据库中所有表,再运行迁移
migrate:install     # 创建迁移存储库,在数据库生成migrations表,记录迁移状态
migrate:refresh     # 重置并重新运行所有迁移,回滚之后再运行迁移
migrate:reset       # 回滚所有数据库迁移,清空数据,删除表.迁移表数据删除,索引保留
migrate:rollback    # 回滚上次数据库迁移,清空数据,删除表.迁移表数据删除,索引保留
migrate:status      # 显示每个迁移的状态
php artisan make:migration # 创建迁移文件
```

#### 更新env数据库相关配置

配置offline.evn和online.env文件 .




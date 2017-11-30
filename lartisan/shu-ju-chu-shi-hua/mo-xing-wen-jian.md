# 模型文件

所有模型移动到`app/Models`目录中 . 全局搜索App\User , 修改替换为`App\Models\User`

commit一下 .

---

#### 创建模型

```
php artisan make:model Models/Article
# -m或--migration表示同时创建迁移文件
php artisan make:model Models/Article -m
php artisan make:model Models/Article --migration
```




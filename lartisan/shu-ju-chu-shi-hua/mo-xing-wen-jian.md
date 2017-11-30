# 模型文件

所有模型移动到`app/Models`目录中 . 全局搜索App\User , 修改替换为`App\Models\User`

commit一下 .

---

#### Eloquent约定

Eloquent模型默认情况下会使用类的「下划线命名法」与「复数形式名称」来作为数据表的名称生成规则 , 例如 : 

* Article 数据模型类对应`articles`表；
* User 数据模型类对应`users`表；
* BlogPost 数据模型类对应`blog_posts`表；

因此 Eloquent 将会假设 Article 模型被存储记录在 articles 数据表中 . 如果你需要指定自己的数据表 , 则可以通过`table`属性来定义 , 例如 : 

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    protected $table = 'my_articles';
}
```

#### 创建模型

```
php artisan make:model Models/Article
# -m或--migration表示同时创建迁移文件
php artisan make:model Models/Post -m
php artisan make:model Models/Post --migration
```




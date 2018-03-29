# 模型文件

所有模型移动到`app/Models`目录中 . 全局搜索App\User , 修改替换为`App\Models\User`

commit一下 .

---

#### Eloquent约定

> Eloquent 提供了简洁优雅的 ActiveRecord 实现来跟数据库进行交互。Active Record 是一种领域模型模式，该模式由 Martin Fowler 在 2003 年出版的《企业应用架构模式》一书中进行了详细叙述并命名。其特点是一个模型类对应关系型数据库中的一个表，模型类的一个实例对应表中的一行记录。Active Record 最大优点是允许我们简单, 直观地操作数据层。

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

#### 约定优于配置

约定优于配置\(convention over configuration\) , 也称作按约定编程 , 这是一种软件设计范式 , 旨在减少软件开发人员需做决定的数量，获得简单的好处 , 而又不失灵活性 . 如果所用工具的约定与你的期待相符 , 便可省去配置 ; 反之 , 你可以配置来达到你所期待的方式 . Eloquent 数据表命名约定机制即属于约定优于配置 .

**常用属性**

* `protected $table = 'users';` - 指明要进行数据库交互的数据库表名称
* `protected $fillable = ['name', 'email', 'password'];` - 过滤用户提交的字段 , 只有包含在该属性中的字段才能够被正常更新
  * 这里的保护举个例子 , 用户在进行表单提交时 , 提交猜测字段 , 例如is\_admin , 以获取管理员权限 . 
* `protected $hidden = ['password', 'remember_token'];` - 需要对用户密码或其它敏感信息在用户实例通过数组或 JSON 显示时进行隐藏

---

#### 创建模型

这里自定义了模型文件的文件夹Models : 

```
$ mkdir app/Models
$ mv app/User.php app/Models/User.php
```

注意别忘记修改已有文件的命名空间 , 可以使用编辑器快速搜索出相关文件修改命名空间 . 

```
php artisan make:model Models/Article
# -m或--migration表示同时创建迁移文件
php artisan make:model Models/BlogPost -m
php artisan make:model Models/BlogPost --migration
```

**编辑迁移文件database/migrations**

```
# 新增4个列
$table->string('slug')->unique(); # 别名,将文章标题转化为URL的一部分,以利于SEO
$table->string('title'); # 文章标题
$table->text('content'); # 文章内容
$table->timestamp('published_at')->index(); # 文章正式发布时间
```

```
php artisan migrate
```

简单的定义模型操作

```
protected $dates = ['published_at'];

public function setTitleAttribute($value)
{
    $this->attributes['title'] = $value;

    if (! $this->exists) {
        $this->attributes['slug'] = str_slug($value);
    }
}
```

#### 添加测试数据

**创建模型工厂**

```
php artisan make:factory BlogPostFactory
```

```
return [
    'title' => $faker->sentence(mt_rand(3, 10)),
    'content' => join("\n\n", $faker->paragraphs(mt_rand(3, 6))),
    'published_at' => $faker->dateTimeBetween('-1 month', '+3 days'),
];
```

**创建播种机**

```
php artisan make:seeder PostTableSeeder
```

```
public function run()
{
    # 清空表中数据
    App\Models\BlogPost::truncate();
    factory(App\Models\BlogPost::class, 100)->create();
}
```

```
public function run()
{
    # 新版本中已经自动禁用了对批量创建数据的保护(fillable等)
    // Model::unguard();
    $this->call('PostTableSeeder');
    // Model::reguard();
}
```




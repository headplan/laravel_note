# Eloquent

### 关联

#### 定义关联

```
一对一:hasOne(),belongsTo()
一对多:hasMany(),belongsTo()
多对多:belongsToMany(),belongsToMany()
远层一对多:hasManyThrough()
多态关联:morphMany(),morphMany(),morphTo()
多对多多态关联:
```

#### **一对一**

```php
public function phone()
{
    return $this->hasOne('Phone::class');
}
```

一个用户模型对应一个手机模型 . 例如一个用户有一个手机号 .

hasOne支持三个参数 :

```php
return $this->hasOne('App\Phone', 'foreign_key', 'local_key');
```

第二个参数 : 外键名 , Eloquent会基于**模型名**决定外键名称 , 这里即user\_id , 也可以自定义其他的 .

第三个参数 : 关联ID名 , Eloquent默认关联id , 也就是user\_id中关联user模型中的id主键 , 定义这个可以关联其他的user模型字段.

现在就可以动态获取了 :

```php
$phone = User::find(1)->phone;
```

**一对一反向关联**

hasOne方法对应的方法belongsto方法定义反向关联 .

```php
public function user()
{
    return $this->belongsTo('User::class');
}
```

Eloquent 会尝试匹配 Phone 模型上的 user\_id 至 User 模型上的 id .

根据关联的方法名 , 这里即user . 拼接字符串`_id`来确定关联的id , 最后拼接为外键名称user\_id .

belongsTo方法支持四个参数 :

```php
public function belongsTo($related, $foreignKey = null, $ownerKey = null, $relation = null)
```

第一个参数即父类命名空间 .

第二个参数是指定外键名称 , 这里是user\_id .

第三个参数是指定要关联的父类的字段 , 可以理解为自定义键 .

第四个参数相当于使用了with参数 , 其中使用其他模型 .

**默认模型**

belongsTo允许使用默认模型 , 这种设计模式通常称为空对象 , 默认返回null , 也可以定义没有找到对应父类内容时的默认值 .

免去了判断的部分 .

使用withDefault\(\)方法 , 其接受数组和闭包的方式 .

```php
public function user()
{
    return $this->belongsTo('App\User')->withDefault();
}
```

```php
public function user()
{
    return $this->belongsTo('App\User')->withDefault(function ($user) {
        $user->name = 'test';
        $user->phone = 123;
    });
}
```

---

#### **一对多**

关联用于定义单个模型拥有任意数量的其它关联模型 , 例如一篇文章有无限多条评论 .

例如在文章Article模型中定义评论Comment关联 .

```php
public function comments()
{
    return $this->hasMany('App\Comment');
}
```

调用方法和参数和hasOne方法一样 , 都是默认以snake+\_id的方式关联父级模型的id主键 .

访问也可以链式操作 .

```php
return $this->hasMany('App\Comment', 'foreign_key', 'local_key');
```

**一对多反向关联**

使用方式和一对一中的一样 , 在子级模型中使用`belongsTo`方法定义它 .

```php
public function post()
{
    return $this->belongsTo('App\Post');
}
```

默认的withDefault\(\)等其他都一样 .

---

#### 多对多

常见的关联关系 , 就是用户与角色的关联 . N个用户可以是管理员 . 这里需要约定 , user表 , role表 , role\_user表 , 其中role\_user是以相关联的两个模型数据表、依照字母顺序排列命名的，并且包含`user_id`和`role_id`字段 .

例如获取用户的所有角色 :

```php
public function roles()
{
    return $this->belongsToMany('App\Role');
}
```

这里获取用户的角色 , 需要使用的是roles\(\)方法 :

```php
$user = App\User::find(1);

foreach ($user->roles as $role) {
    //
}
```

依然可以使用链式操作 , 进行条件约束 :

```php
$roles = App\User::find(1)->roles()->orderBy('name')->get();
```

belongsToMany方法中有4个参数 , 第一个就是要关联的模型 , 后三个参数分别自定义中间表名 , 已经两个关联的键名 :

```php
return $this->belongsToMany('App\Role', 'role_user', 'user_id', 'role_id');
```

**定义反向关联**

反向关联同上 , 使用方式都一样 .

**获取中间表字段**

使用pivot属性访问中间表的数据 , 就像一个模型一样使用 .

```php
$user = App\User::find(1);

foreach ($user->roles as $role) {
    echo $role->pivot->created_at;
}
```

关联表一般只有俩个字段 , 就是前面的user\_id和role\_id , 如果还有其他字段 , 需要在定义关联时候指定一下 .

```php
return $this->belongsToMany('App\Role')->withPivot('column1', 'column2');
return $this->belongsToMany('App\Role')->withTimestamps(); # 表示指定了时间戳
```

**通过中间表过滤关联数据**

定义时时候wherePivot和wherePivotIn方法 .

```php
return $this->belongsToMany('App\Role')->wherePivot('approved', 1);

return $this->belongsToMany('App\Role')->wherePivotIn('priority', [1, 2]);
```

**定义自定义中间表模型**

前面都是在关联模型时 , 使用pivot等方法对关联表进行操作 , 还可以继承Pivot类 , 直接定义个关联表模型 , 在belongsToMany后面使用using表示使用 :

```php
public function users()
{
    return $this->belongsToMany('App\User')->using('App\UserRole');
}
```

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Relations\Pivot;

class UserRole extends Pivot
{
    //
}
```

---

#### 远层一对多

通过一个例子来看一下 , 远层是什么意思 . 例如，一个`Country`模型可以通过中间的`User`模型获得多个`Post`模型 .

```
countries
    id - integer
    name - string

users
    id - integer
    country_id - integer
    name - string

posts
    id - integer
    user_id - integer
    title - string
```

这里 , 我们要做的是 , 获取指定国家的所有博客文章 .

虽然 posts 表中不包含 country\_id 字段，但 hasManyThrough 关联能让我们通过 $country-&gt;posts 访问到一个国家下所有的用户文章。为了完成这个查询，Eloquent 会先检查中间表 users 的 country\_id 字段，找到所有匹配的用户 ID 后，使用这些 ID，在 posts 表中完成查找。

在Country模型中定义关联 :

```php
/**
 * 获得某个国家下所有的用户文章。
 */
public function posts()
{
    return $this->hasManyThrough('App\Post', 'App\User');
}
```

hasManyThroughf方法的第一个参数 , 为最终要访问的模型 , 第二个参数是中间经过的模型 .

通常就不用其他参数了 , 因为外键按照格式约定好了, 如果需要自定义 , 也是可以的 :

```php
class Country extends Model
{
    public function posts()
    {
        return $this->hasManyThrough(
            'App\Post',
            'App\User',
            'country_id', // 用户表外键...
            'user_id', // 文章表外键...
            'id', // 国家表本地键...
            'id' // 用户表本地键...
        );
    }
}
```

---

#### 多态关联

允许一个模型在单个关联上属于多个其他模型 . 举个简单的例子就是 , 用户既可以评论文章 , 又可以评论视频 . 就是使用一个comments评论表 , 同时满足两个使用场景 . 看一个简单的表结构 :

```
posts
    id - integer
    title - string
    body - text

videos
    id - integer
    title - string
    url - string

comments
    id - integer
    body - text
    commentable_id - integer
    commentable_type - string
```

文章 , 视频 , 评论 , 这里要注意的是评论表中的commentable\_id和commentable\_type . `commentable_id`用来保存视频和文章的ID值 . `commentable_type`用来保存所属模型的类名 . 这样就达到了多态的效果 . 接下来直接看一下这种多态的方式 , 模型中应该怎样定义 :

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * 获得拥有此评论的模型。
     */
    public function commentable()
    {
        return $this->morphTo();
    }
}

class Post extends Model
{
    /**
     * 获得此文章的所有评论。
     */
    public function comments()
    {
        return $this->morphMany('App\Comment', 'commentable');
    }
}

class Video extends Model
{
    /**
     * 获得此视频的所有评论。
     */
    public function comments()
    {
        return $this->morphMany('App\Comment', 'commentable');
    }
}
```

再来看一下具体参数 :

```php
public function morphMany($related, $name, $type = null, $id = null, $localKey = null)
```

第一个参数 , 看英文是相关的 , 这里也就是comment模型 .

第二个参数 , 名字即访问评论模型时的属性名 , 也就是函数名 .

后面三个参数 , 即是指定comments表中自定义的commentable\_id和commentable\_type字段 , 最后一个就是本地key , 就是Post或者Video的id .

**获取多态关联**

数据库表准备好、模型定义完成后 , 就可以直接访问获取关联数据 .

```php
$post = App\Post::find(1);

foreach ($post->comments as $comment) {
    //
}
```

还可以通过访问调用了`morphTo`的关联方法获得多态关联的拥有者 .

```php
$comment = App\Comment::find(1);

$commentable = $comment->commentable;
```

这里会根据类型返回会返回`Post`或者`Video`实例 .

**自定义多态关联的类型字段**

默认 , Laravel 会使用完全限定类名作为关联模型保存在多态模型上的类型字段值 . 根据前面的例子 , 其意思就是Comment 属于 Post 或者 Video , 那么 commentable\_type 的默认值对应地就是 App\Post 和 App\Video . 这是完全限定的 . 为了解耦 , 可以在AppServiceProvider中 , 初始化时候添加一个自定义的多态映射表 .

```php
use Illuminate\Database\Eloquent\Relations\Relation;

Relation::morphMap([
    'posts' => 'App\Post',
    'videos' => 'App\Video',
]);
```

这样 , 存在commentable\_type中的就是posts和videos , 就不是直接存的类名了 . 当然也可以使用一个独立的服务提供者注册 .

---

#### 多对多多态关联

这里最好的理解方式 , 就是Tag标签的概念 . 例如 , Post 模型和 Video 模型可以共享一个多态关联至 Tag 模型 . 使用多对多多态关联可以让您在文章和视频中共享唯一的标签列表 . 先来看看数据表结构 :

```
posts
    id - integer
    name - string

videos
    id - integer
    name - string

tags
    id - integer
    name - string

taggables
    tag_id - integer
    taggable_id - integer
    taggable_type - string
```

然后在模型上定义关联关系 :

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * 获得此文章的所有标签。
     */
    public function tags()
    {
        return $this->morphToMany('App\Tag', 'taggable');
    }
}
```

定义反向关联 : 

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    /**
     * 获得此标签下所有的文章。
     */
    public function posts()
    {
        return $this->morphedByMany('App\Post', 'taggable');
    }

    /**
     *  获得此标签下所有的视频。
     */
    public function videos()
    {
        return $this->morphedByMany('App\Video', 'taggable');
    }
}
```




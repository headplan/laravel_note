# 更新父级时间戳

当一个模型belongsTo\(belongsToMany\)另一个模型时 , 比如一个Comment属于一个Post , 需要在更新子模型时候更新其父模型时间戳 . 就是当 , Comment模型更新时 , 自动触发其父级Post模型的updated\_at时间戳也更新 , 只需要在子模型中添加一个包含关联名称的touches属性即可开启 :

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * 所有会被触发的关联。
     *
     * @var array
     */
    protected $touches = ['post'];

    /**
     * 获得此评论所属的文章。
     */
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
```

配置即可 , 现在更新Comment时候 , 就会更新Post中的updated\_at字段了 , 也更方便的知道 , 什么时候让Post的缓存失效 : 

```php
$comment = App\Comment::find(1);

$comment->text = '编辑了这条评论！';

$comment->save();
```




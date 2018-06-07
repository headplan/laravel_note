# 查询关联

所有类型的关联都是通过方法定义的 , 可以调用方法来获取关联实例 , 然后在其后使用查询语句构造器进行约束 .

```php
return $test->posts()->first();
# 和直接返回属性一样
return $test->posts;
```

加一些条件约束 :

```php
$user = App\User::find(1);

$user->posts()->where('active', 1)->get();
```

#### 关联方法和动态属性

前面提到过 , 如果没什么条件约束 , 可以直接使用属性的方式访问数据 , 即动态属性 . 动态属性都是懒加载的 , 即这些关联数据在实际被访问时才被加载 . 这也引出了后面的预加载的意义 , 为了减少SQL语句请求 , 这些关联的数据经常需要预加载 .

#### 基于存在的关联查询

举个例子 , 想获取至少有一条评论的所有博客文章 . 可以给has方法传递关联名称 : 

```php
# 获取所有至少有一条评论的文章
$posts = App\Post::has('comments')->get();
```

还可以指定条件 : 

```php
# 获取所有有三条或三条以上评论的文章...
$posts = Post::has('comments', '>=', 3)->get();
```

也可以使用`点`符号构造嵌套的has语句 , 例如 , 想获得至少有一条获赞评论的文章 : 

```php
$posts = Post::has('comments.votes')->get();
```

has方法的扩展 , 即可以使用闭包的方式设置where条件 . 即whereHas方法和orWhereHas方法 . 

```php
# 获得所有至少有一条评论内容满足foo%条件的文章
$posts = Post::whereHas('comments', function ($query) {
    $query->where('content', 'like', 'foo%');
})->get();
```

#### 基于不存在的关联查询

#### 关联数据计数




# 粉丝关系

#### 创建关系表

在关注用户功能的整个流程中 , 最重要的两个主体分别是被关注的用户\(user\_id\)和粉丝\(follower\_id\) , 我们可以通过被关注用户\(user\_id\)来获取到他所有的粉丝 , 也能通过一个粉丝\(follower\_id\)来获取到他关注的所有用户 .

```php
Schema::create('followers', function (Blueprint $table) {
    $table->increments('id');
    $table->integer('user_id')->index();
    $table->integer('follower_id')->index();
    $table->timestamps();
});
```

执行迁移命令 .

#### 关联关系

在 Laravel 中我们使用`belongsToMany`来关联模型之间的多对多关系 :

```php
/**
 * 操作用户的关注者集合
 * @return \Illuminate\Database\Eloquent\Relations\BelongsToMany
 */
public function followers()
{
    return $this->belongsToMany(User::class, 'followers', 'user_id', 'follower_id');
}

/**
 * 操作用户关注的集合
 * @return \Illuminate\Database\Eloquent\Relations\BelongsToMany
 */
public function followings()
{
    return $this->belongsToMany(User::class, 'followers', 'follower_id', 'user_id');
}
```

这里`belongsToMany`的四个参数的含义分别是 , 用户模型 , 自定义关联关系表名\(默认为followers\_user,字母排序的合并\) , 在关联中的模型外键 , 要合并的模型外键 . 

为用户和粉丝模型进行了多对多关联之后 , 便可以使用 Eloquent 模型为多对多提供的一系列简便的方法了 . 

> 使用`attach`方法或`sync`方法在中间表上创建一个多对多记录 , 使用`detach`方法在中间表上移除一个记录 , 创建和移除操作并不会影响到两个模型各自的数据 , 所有的数据变动都在**中间表**上进行 . `attach,sync,detach`这几个方法都允许传入 id 数组参数 .

可以使用tinker测试一下 . 




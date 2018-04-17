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

可以使用tinker测试一下 :

```php
# 这里让 id 为 1 的用户去关注 id 为 2 和 id 为 3 的用户
# 获取id为1的用户模型
>>> $user = App\Models\User::find(1);
# 操作其关注id2和id3的用户
>>> $user->followings()->attach([2, 3]);
# 获取用户关注的用户id
>>> $user->followings()->allRelatedIds()->toArray()

# 重复关注问题
>>> $user->followings()->attach([2]);
>>> $user->followings()->allRelatedIds()->toArray();

# 使用sync方法解决,并且第二个参数为false,不移除之前添加的数据,true为移除之前数据,以现在的数据为准.
>>> $user->followings()->sync([3], false);

# 取消/删除id数据
>>> $user->followings()->detach([2,3]);
>>> $user->followings()->allRelatedIds()->toArray();
```

使用followings方法 , 完成关注和取消关注两个操作就很简单了 : 

```php
/**
 * 关注
 * @param $user_ids
 */
public function follow($user_ids)
{
    if (!is_array($user_ids)) {
        $user_ids = compact('user_ids');
    }
    $this->followings()->sync($user_ids, false);
}

/**
 * 取消关注
 * @param $user_ids
 */
public function unfollow($user_ids)
{
    if (!is_array($user_ids)) {
        $user_ids = compact('user_ids');
    }
    $this->followings()->detach($user_ids);
}
```




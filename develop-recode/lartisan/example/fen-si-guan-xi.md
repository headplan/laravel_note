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

还需要一个方法用于判断当前登录的用户 A 是否关注了用户 B , 代码实现逻辑很简单 , 只需要判断用户 B 是否包含在用户 A 的关注人列表上即可 :

```php
/**
 * 判断登录用户是否关注
 * @param $user_id
 * @return mixed
 */
public function isFollowing($user_id)
{
    return $this->followings->contains($user_id);
}
```

这里需要注意的是 ,`$this->followings`与`$this->followings()`调用时返回的数据是不一样的 . 前者返回的是Eloquent集合 , 后者返回的是数据库请求构建器 .

在模型里定义了关联方法`followings()`关联关系定义好后 , 可以通过访问`followings`属性直接获取到关注用户的**集合 . **这是 Laravel Eloquent 提供的「动态属性」属性功能 , 可以像在访问模型中定义的属性一样 , 来访问所有的关联方法 , 重点返回的是集合 , 而上面代码中contains\(\)即是操作集合的方法 . 所以 , 可以理解为

```php
$user->followings == $user->followings()->get() // 等于 true
```

#### 演示数据

```
$ php artisan make:seeder FollowersTableSeeder
```

> 这里没有要生成的随机内容 , 所以不用创建Factory了 .

```php
public function run()
{
    $users = User::all();
    $user = $users->first();
    $user_id = $user->id;

    # 获取去除掉 ID 为 1 的所有用户 ID 数组
    $followers = $users->slice(1);
    $follower_ids = $followers->pluck('id')->toArray();

    # 关注除了ID为1以外的所有用户
    $user->follow($follower_ids);

    # 除了ID为1的用户以外,所有用户都关注ID为1的用户
    foreach ($followers as $follower) {
        $follower->follow($user_id);
    }
}
```

> 别忘记添加到DatabaseSeeder中 .

重置并填充数据 :

```
$ php artisan migrate:refresh --seed
```

#### 关注的人列表和粉丝列表

**定义路由**

```php
# 显示用户的关注人列表
Route::get('/users/{user}/followings','UsersController@followings')->name('users.followings');
# 显示用户的粉丝列表
Route::get('/users/{user}/followers','UsersController@followers')->name('users.followers');
```

**添加方法**

这里默认已经被中间件拦截为登录用户才能访问 . 直接添加方法就可以了 :

```php
/**
 * 关注的人列表
 * @param \App\Models\User $user
 * @return \Illuminate\Contracts\View\Factory|\Illuminate\View\View
 */
public function followings(User $user)
{
    $users = $user->followings()->paginate(30);
    $title = '关注的人';
    return view('users.show_follow', compact('users', 'title'));
}

/**
 * 粉丝列表
 * @param \App\Models\User $user
 * @return \Illuminate\Contracts\View\Factory|\Illuminate\View\View
 */
public function followers(User $user)
{
    $users = $user->followers()->paginate(30);
    $title = '粉丝';
    return view('users.show_follow', compact('users', 'title'));
}
```

**创建视图**

视图是前面两个列表公用的 , 前面的变量名也是相同的 :

```php
@extends('layouts.default')
@section('title', $title)

@section('content')
    <div class="col-md-offset-2 col-md-8">
        <h1>{{ $title }}</h1>
        <ul class="users">
            @foreach($users as $user)
                <li>
                    <img src="{{ $user->gravatar() }}" alt="{{ $user->name }}" class="gravatar">
                    <a href="{{ route('users.show', $user->id) }}" class="username">{{ $user->name }}</a>
                </li>
            @endforeach
        </ul>

        {!! $users->links('vendor.pagination.simple-default') !!}
    </div>
@stop
```

再添加一个include视图 , 用来展示用户的关注人数、粉丝数、微博发布数等 , 这里直接使用count方法计算人数 , 没有使用计数器冗余字段 .

```php
<div class="stats">
    <a href="{{ route('users.followings', $user->id) }}">
        <strong id="followings" class="stats">
            {{ count($user->followings) }}
        </strong>
        关注
    </a>
    <a href="{{ route('users.followers', $user->id) }}">
        <strong id="followers" class="stats">
            {{ count($user->followers) }}
        </strong>
        粉丝
    </a>
    <a href="{{ route('users.statuses', $user->id) }}">
        <strong id="statuses" class="stats">
            {{ count($user->statuses()->count()) }}
        </strong>
        微博
    </a>
</div>
```

然后引入到home页中 . 调整样式 .

#### 关注与取消关注功能

**创建路由**

```php
# 关注用户
Route::post('/users/followers/{user}', 'FollowersController@store')->name('followers.store');
# 取消关注
Route::delete('/users/followers/{user}', 'FollowersController@destroy')->name('followers.destroy');
```

**创建控制器**

```
php artisan make:controller FollowersController
```

**添加视图**

```php
@if($user->id !== Auth::user()->id)
    <div id="follow_form">
        @if(Auth::user()->isFollowing($user->id))
            <form action="{{ route('followers.destroy', $user->id) }}" method="post">
                {{ csrf_field() }}
                {{ method_field('DELETE') }}
                <button type="submit" class="btn btn-sm">取消关注</button>
            </form>
        @else
            <form action="{{ route('followers.store', $user->id) }}" method="post">
                {{ csrf_field() }}
                <button type="submit" class="btn btn-sm btn-primary">关注</button>
            </form>
        @endif
    </div>
@endif
```

这里判断了当用户访问自己的个人页面时 , 不显示关注按钮 :

```php
$user->id !== Auth::user()->id
```

后面还判断了用户对访问用户的关注状态 :

```php
Auth::user()->isFollowing($user->id)
```

现在将关注数等信息和按钮添加到个人页面 :

```php
@if(Auth::check())
    @include('users.follow')
@endif
```

这里还判断了用户是否登录 . 然后更新关注和取关控制器方法 :

```php
/**
 * 关注
 * @param \App\Models\User $user
 * @return \Illuminate\Http\RedirectResponse|\Illuminate\Routing\Redirector
 */
public function store(User $user)
{
    if (Auth::user()->id === $user->id) {
        return redirect('/');
    }

    if (!Auth::user()->isFollowing($user->id)) {
        Auth::user()->follow($user->id);
    }

    return redirect()->route('users.show', $user->id);
}

/**
 * 取消关注
 * @param \App\Models\User $user
 * @return \Illuminate\Http\RedirectResponse|\Illuminate\Routing\Redirector
 */
public function destroy(User $user)
{
    if (Auth::user()->id === $user->id) {
        return redirect('/');
    }

    if (Auth::user()->isFollowing($user->id)) {
        Auth::user()->unfollow($user->id);
    }

    return redirect()->route('users.show', $user->id);
}
```

#### 动态流

在主页上显示所有关注用户的微博动态 . 修改User模型中 , 原来的feed方法 , 查找所有关注的人和自己的微博 :

```php
public function feed()
{
    $user_ids = Auth::user()->followings->pluck('id')->toArray();
    array_push($user_ids, Auth::user()->id);
    return Status::whereIn('user_id', $user_ids)
        ->with('user')
        ->orderBy('created_at', 'desc');
}
```




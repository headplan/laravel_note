# 微博CRUD

#### 微博模型

创建迁移文件

```
php artisan make:migration create_statuses_table --create=statuses
```

编辑迁移文件 , 添加字段 :

```php
# 存储微博内容
$table->text('content');
# 存储微博发布者id,添加索引
$table->integer('user_id')->index();
# 给微博创建时间添加索引
$table->index(['created_at']);
```

执行迁移

```
$ php artisan migrate
```

创建微博模型文件

```
$ php artisan make:model Models/Status
```

**用户和微博之间的关系**

Eloquent 模型让关联的管理和处理变得更加简单 , 支持 :

* 一对一
* 一对多
* 多对多
* 远层一对多
* 多态关联
* 多态多对多关联

以`一对多`为例子 , 在模型中将 Eloquent 关联定义为函数 :

_app/Models/Status.php_

```php
public function user()
{
    return $this->belongsTo(User::class);
}
```

在用户模型中 , 指明一个用户拥有多条微博 :

_app/Models/User.php_

```php
public function statuses()
{
    return $this->hasMany(Status::class);
}
```

#### 显示微博

为了后面方便测试 , 先生成假数据 :

```
php artisan make:factory StatusFactory
php artisan make:seeder StatusesTableSeeder
```

```php
$factory->define(\App\Models\Status::class, function (Faker $faker) {
    $date_time = $faker->date . ' ' . $faker->time;

    return [
        'content' => $faker->text(),
        'created_at' => $date_time,
        'updated_at' => $date_time,
    ];
});
```

```php
public function run()
{
    //$user_ids = ['1', '2', '3'];
    $users = User::where('id', '<', 4)->get();
    $user_ids = $users->map(function ($user) {
        return $user->id;
    })->toArray();

    $faker = app(Faker\Generator::class);

    $statuses = factory(Status::class)->times(100)->make()->each(function ($status) use ($faker, $user_ids) {
        $status->user_id = $faker->randomElement($user_ids);
    });

    Status::insert($statuses->toArray());
}
```

别忘记最后的 :

```php
$this->call(StatusesTableSeeder::class);
```

执行命令 :

```
$ php artisan migrate:refresh --seed
```

#### 展示微博

在用户控制器的show方法中 , 取出改用户的所有微博 , 前面在模型上我们已经做了关联 , 所以取出数据变得非常方便 :

```php
$statuses = $user->statuses()
    ->orderByDesc('created_at')
    ->paginate(30);
```

视图中渲染数据 :

```php
<li id="status-{{ $status->id }}">
    <a href="{{ route('users.show', $user->id) }}">
        <img src="{{ $user-gravatar() }}" alt="{{ $user->name }}" class="gravatar">
    </a>
    <span class="user">
        <a href="{{ route('users.show', $user->id) }}">{{ $user->name }}</a>
    </span>
    <span class="timestamp">{{ $status->created_at->diffForhumans() }}</span>
    <span class="content">{{ $status->content }}</span>
</li>
```

其中 , 用到了一个diffForHumans\(\)方法 , 把时间转化为友好的显示方式 . 之后还要对Carbon类进行本地化设置 :

> [Carbon](https://github.com/briannesbitt/Carbon) 是 PHP DateTime 的一个简单扩展 , Laravel 将其默认集成到了框架中 .

```
public function boot()
{
    Carbon::setLocale('zh');
}
```

在AppServiceProvider设置启动时加载 .

引入微博视图 , 展示当前用户的所有微博 :

```php
<div class="col-md-12">
    @if(count($statuses) > 0)
        <ol class="statuses">
            @foreach($statuses as $status)
                @include('includes.status')
            @endforeach
        </ol>
        {!! $statuses->links('vendor.pagination.simple-default') !!}
    @endif
</div>
```

最后更新一下样式 .

**微博相关操作**

创建路由 , 这里同样创建资源路由 , 但只使用其中的两个 , 创建和删除动作 :

```php
Route::resource('statuses', 'StatusesController', ['only' => ['store', 'destroy']]);
```

创建控制器

```
php artisan make:controller StatusesController
```

添加auth中间件

```php
public function __controller()
{
    $this->middleware('auth');
}
```

发布微博操作

```php
public function store(Request $request)
{
    # 过滤数据
    $this->validate($request, [
        'content' => 'required|max:140'
    ]);

    # 获取用户模型并关联微博数据创建
    Auth::user()->statuses()->create([
        'content' => $request->content
    ]);

    # 重定向
    return redirect()->back();
}
```

创建微博发布表单页面

```php
<form action="{{ route('statuses.store') }}" method="POST">
    @include('includes.errors')
    {{ csrf_field() }}
    <textarea name="content" class="form-control" placeholder="聊聊新鲜事儿..." cols="30" rows="10">{{ old('content') }}</textarea>
    <button type="submit" class="btn btn-primary pull-right">发布</button>
</form>
```




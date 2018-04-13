# 用户操作

**更新用户**

前面用户展示已经使用了resource资源路由 , 不用添加编辑路由了

```php
Route::get('/users/{user}/edit', 'UsersController@edit')->name('users.edit');
```

这里也使用了隐性路由模型绑定 , 控制器直接写即可

```php
public function edit(User $user)
{
    return view('users.edit', compact('user'));
}
```

添加edit模板 , 几个注意的点 :

```
<form method="POST" action="{{ route('users.update', $user->id )}}">
# 转换后为
<form method="POST" action="http://lartisan.example/users/1">
# 伪造的更新资源请求,隐藏域
{{ method_field('PATCH') }}
# 转换后
<input type="hidden" name="_method" value="PATCH">
# 禁止修改的表单
<input type="text" name="email" class="form-control" value="{{ $user->email }}" disabled>
```

修改app.scss , 添加点样式 , 然后修改header头的入口链接 .

添加update控制器方法 , 基本的逻辑是 :

首先validate验证表单数据格式 , 验证成功后 , 更新数据 .

这里的密码部分如果没有修改 , 也就是当用户提供空白密码时 , 赋值原来的密码 , 然后闪存消息提示状态 :

```php
public function update(User $user, Request $request)
{
    $this->validate($request, [
        'name' => 'required|min:3|max:50',
        'password' => 'nullable|min:6|max:255|confirmed'
    ]);

    $data = [];
    $data['name'] = $request->name;
    if ($request->password) {
        $data['password'] = bcrypt($request->password);
    }
    $user->update($data);

    session()->flash('success', '个人资料更新成功!');
    return redirect()->route('users.show', $user->id);
}
```

#### 操作权限

现在面临的两个问题是 , 未登录状态也可以访问edit和update两个操作 , 登录用户也可以操作别人的个人信息 .

**中间件过滤**

Laravel 中间件 \(Middleware\)提供了一种非常棒的过滤机制来过滤进入应用的 HTTP 请求 , 中间件文件都存放在`app/Http/Middleware`文件夹中 , Laravel也内置了很多中间件 , 身份验证、CSRF 保护等 . 这里我们使用 Auth 中间件来验证用户的身份时 , 如果用户未通过身份验证 , 则 Auth 中间件会把用户重定向到登录页面 , 通过了继续往下进行 .

$this-&gt;middleware\(\)的两个参数 , 分别是要使用的中间件和要过滤的操作 , 可以使用except添加黑名单 , 也可以使用其相反的白名单机制only .

```php
public function __construct()
{
    $this->middleware('auth', [
        'except' => ['show', 'create', 'store']
    ]);
}
```

**授权策略Policy**

使用授权策略 \(Policy\)来对用户的操作权限进行验证 , 在用户未经授权进行操作时将返回 403 禁止访问的异常 , 让用户只能编辑自己的资料 . 创建策略 :

```
php artisan make:policy UserPolicy
```

生成的策略文件在`app/Policies`文件夹下 , 为其添加方法用于用户更新时的权限验证 :

```php
/**
 * id相同表示为相同用户,用户通过授权,可以接着进行下一个操作.
 * @param \App\Models\User $currentUser 当前登录用户实例
 * @param \App\Models\User $user 要进行授权的用户实例
 * @return bool
 */
public function update(User $currentUser, User $user)
{
    return $currentUser->id === $user->id;
}
```

使用授权策略需要注意 :

* 并不需要检查`$currentUser`是不是 NULL . 未登录用户 , 框架会自动为其**所有权限**返回 . 
* 调用时也**不需要**传递当前登录用户至该方法内 , 框架会自动加载当前登录用户 . 

接下来 , 还需要在`AuthServiceProvider`类中对授权策略进行设置 , 其中包含了`policies`属性 , 用于将各种模型对应到管理它们的授权策略上 . 现在我们要做的就是为用户模型`User`指定授权策略`UserPolicy`.

```php
protected $policies = [
    'App\Model' => 'App\Policies\ModelPolicy',
    \App\Models\User::class => \App\Policies\UserPolicy::class,
];
```

定义了授权策略之后 , 就可以在用户控制器中使用`authorize`方法来验证用户授权策略 . `authorize`方法接收两个参数 , 第一个为授权策略的名称 , 第二个为进行授权验证的数据 .

> 默认的`App\Http\Controllers\Controller`类包含了 Laravel 的`AuthorizesRequests`trait . 此 trait 提供了`authorize`方法 , 它可以被用于快速授权一个指定的行为 , 当无权限运行该行为时会抛出 HttpException .

```php
$this->authorize('update', $user);
```

> 这里`update`是指授权类里的`update`授权方法 , `$user`对应传参`update`授权方法的第二个参数 . 正如上面定义`update`授权方法时候提起的 , 调用时 , 默认情况下 , **不需要**传递第一个参数 , 也就是当前登录用户至该方法内 , 因为框架会自动加载当前登录用户 .

接下来将上面的代码添加到需要授权的位置 , 也就是UserController的edit和update方法中 .

```php
public function edit(User $user)
{
    $this->authorize('update', $user);
    return view('users.edit', compact('user'));
}
...
```

现在去编辑其他用户的页面 , 就会报错403 .

**更好的跳转**

将用户重定向到他之前尝试访问的页面 , 也就是那些需要登录才能访问的页面 .

`redirect()`实例提供了一个`intended`方法 , 该方法可将页面重定向到上一次请求尝试访问的页面上 , 并接收一个默认跳转地址参数 , 当上一次请求记录为空时 , 跳转到默认地址上 .

```php
return redirect()->intended(route('users.show', [Auth::user()]));
```

**注册与登录页面访问限制**

还有一个小问题 , 就是已登录用户还能够对注册页面和登录页面进行访问 . Auth中间件提供了auth以登录用户的控制 , 还有对未登录用户的控制 , 就是guest属性 , 可以只让未登录用户访问登录页面和注册页面 .

下面添加一下中间件 , 只让未登录用户访问登录页面和注册页面 :

```php
$this->middleware('guest', [
    'only' => ['create']
]);
```

现在已经调用了`guest`中间件 , 但是其默认跳转`/home`页面 , 这里可以简单的修改一下 , `RedirectIfAuthenticated`中间件类 :

```php
session()->flash('info', '您已登录,无需再次操作.');
return redirect('/');
```

#### 列出所有用户

用户列表对应的控制器是index , 并允许游客访问 :

```php
public function index()
{
    $users = User::all();
    return view('users.index', compact('users'));
}
```

这里一次性查出全部数据 , 可能会有点问题 , 这里一会再说 , 先添加页面 , 样式 , 头部的入口连接 .

> 更新样式时总会缓存 , 这里在webpack.mix.js结尾加上.version\(\) , blade模板引入函数换成mix\(\)即可 .

#### 数据填充

在添加分页前 , 还需要添加一些演示数据 , 可以使用Laravel提供的数据填充来批量生成假用户 .

假数据的生成为分为两个阶段 :

* 模型工厂 - 对要生成假数据的模型指定字段进行赋值
* 数据填充 - 批量生成假数据模型

**模型工厂**

Laravel 默认为我们集成了Faker扩展包 , 使用该扩展包可以让我们很方便的生成一些假数据 .

> [https://github.com/fzaninotto/Faker](https://github.com/fzaninotto/Faker)

Faker的简单使用

```php
// 使用 factory 来创建一个 Faker\Generator 实例
>>> $faker = Faker\Factory::create();
=> Faker\Generator {#748}

// 生成用户名
>>> $faker->name;
=> "Donavon Harber I"

// 生成安全邮箱
>>> $faker->safeEmail;
=> "gibson.ardith@example.org"

// 生成随机日期
>>> $faker->date;
=> "1992-09-19"

// 生成随机时间
>>> $faker->time
=> "11:32:14"
```

借助 Faker 和 Eloquent 模型工厂来为指定模型的每个字段设置随机值 , 定义一下模型工厂 , 可以使用命令行创建 :

```
php artisan make:factory UserFactory
```

默认Laravel为我们准备好了一个模型工厂 , 这里稍作修改 , 让创建时间随机生成 , 配合Faker使用 :

```php
<?php
use Faker\Generator as Faker;

$factory->define(App\Models\User::class, function (Faker $faker) {
    $date_time = $faker->date . ' ' . $faker->time;
    static $password;

    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => $password ?: $password = bcrypt('123456'),
        'remember_token' => str_random(10),
        'created_at' => $date_time,
        'updated_at' => $date_time,
    ];
});
```

**数据填充**

在 Laravel 中使用`Seeder`类来给数据库填充测试数据 . 所有的 Seeder 类文件都放在`database/seeds`目录下 , 文件名需要按照『驼峰式』来命名 , 且严格遵守大小写规范 .

默认会有一个DatabaseSeeder类 , 用来统一控制数据填充的顺序 .

创建一个Seeder :

```
php artisan make:seeder UsersTableSeeder
```

编辑类总的run方法 :

```php
public function run()
{
    $fakeusers = factory(User::class)->times(100)->make();
    User::insert($fakeusers->makeVisible(['password', 'remember_token'])->toArray());

    $user = User::find(1);
    $user->name = 'headplan';
    $user->email = 'headplan@163.com';
    $user->password = bcrypt('123123');
    $user->save();
}
```

先看一下这行代码

```php
factory(User::class)->times(100)->make();
```

factory\(\)是一个helper函数 , 加载了Factory类 , 通过of\(\)方法实例化FactoryBuilder类 , 调用了其api , times和make .

* `times`接受一个参数用于指定要创建的模型数量
* `make`方法调用后将为模型创建一个集合

接着使用了模型的`insert`方法来将生成假用户列表数据批量插入到数据库中 , 这里用到了makeVisible方法 , 用来临时显示User模型里指定的隐藏属性`$hidden`.

后面的内容就是为了id1的用户保持不变 , 更新了一下数据 .

配置DatabaseSeeder类 :

```php
public function run()
{
    Model::unguard();
    $this->call(UsersTableSeeder::class);
    Model::reguard();
}
```

这里的Model::unguard\(\)和Model::reguard\(\)是一对 , 解除和恢复自动填充操作限制的方法 .

命令行生成假数据

```
$ php artisan migrate:refresh
$ php artisan db:seed

# 指定UsersTableSeeder生成
$ php artisan migrate:refresh
$ php artisan db:seed --class=UsersTableSeeder

# 一条命令完成重置和填充
$ php artisan migrate:refresh --seed
```

#### 分页

修改控制器 , 添加分页方法 :

```php
$users = User::paginate(10);
```

然后在页面上渲染 :

```php
{!! $users->links() !!}
```

这里是`{!! !!}`而不是`{{ }}`是为了防止URL被转义 .

重新调整一下列表页布局 , 把每个用户的样式抽取出来 , include调用 .

```php
@foreach($users as $user)
    @include('includes.user')
@endforeach
```

> 分页视图可以自定义 , 使用命令生成默认的模板 , 就可以自定义了 , 具体可以查看手册 .
>
> ```php
> php artisan vendor:publish --tag=laravel-pagination
> ```
>
> ```php
> {!! $users->links('vendor.pagination.simple-default') !!}
> ```

#### 删除用户

将管理员身份授权给某个指定用户 . 

生成迁移文件为用户表新增管理员字段 , 使用`--table`选项可以为指定数据表生成迁移文件 : 

```
php artisan make:migration add_is_admin_to_users_table --table=users
```

修改迁移文件 : 

```php
public function up()
{
    Schema::table('users', function (Blueprint $table) {
        $table->boolean('is_admin')->default(false);
    });
}

public function down()
{
    Schema::table('users', function (Blueprint $table) {
        $table->dropColumn('is_admin');
    });
}
```




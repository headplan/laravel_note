# 用户模块

#### 注册登录

**生成代码**

```
php artisan make:auth
```

`make:auth`会询问我们是否要覆盖`app.blade.php`这里选择no

可以使用`git status`查看文件更改状态 .

**查看新增的路由**

```
php artisan route:list
```

**删除修改生成的多余内容**

```
# 多余的路由绑定
Route::get('/home', 'HomeController@index')->name('home');
# 多余的控制器和模板
$ rm resources/views/home.blade.php
# 控制修改回原来加载的模板
return view('home.index');
# 中间件暂时排除index
->except('index');
```

**修改顶部模板**

```
# 两处路由别名修改
<li><a href="{{ route('login') }}">登录</a></li>
<li><a href="{{ route('register') }}">注册</a></li>

# 然后是添加判断,这里直接判断未登录用户和登录用户展示的页面内容
@guest
    <li><a href="{{ route('login') }}">登录</a></li>
    <li><a href="{{ route('register') }}">注册</a></li>
@else
    .
    .
    .
@endguest

# 还要注意下退出登录时候的CSRF令牌,退出用js事件去提交
onclick="event.preventDefault();document.getElementById('form-logout').submit()
<form id="logout-form" action="{{ route('logout') }}" method="POST" style="display: none;">
    {{ csrf_field() }}
</form>
```

**执行迁移**

```
php artisan migrate
```

现在基本的注册登录已经可以使用 , 修改登录注册中的跳转地址 :

```
protected $redirectTo = '/';
modified:   app/Http/Controllers/Auth/LoginController.php
modified:   app/Http/Controllers/Auth/RegisterController.php
modified:   app/Http/Controllers/Auth/ResetPasswordController.php
modified:   app/Http/Middleware/RedirectIfAuthenticated.php
```

#### **整体模板修改**

```
# 基本上是注册登录逻辑的四个模板
login.blade.php
register.blade.php
email.blade.php
reset.blade.php
```

**login.blade.php**

```
# 修改模板页面
# 主要是路由连接地址和错误提示
{{ route('password.request') }}
{{ $errors->has('password') ? ' is-danger' : '' }}
# 登录状态使用了组件b-checkbox
# 简单引入极验验证,流程查看扩展包使用下属文章,暂时未引入
```

**register.blade.php**

```
# 添加了注册验证码,直接调用开发包,后边JS随机个数切换验证码
<img class="captcha" src="{{ captcha_src('flat') }}" 
    onclick="this.src='/captcha/flat?'+Math.random()" title="点击图片重新获取验证码">
```

重写控制器中对注册的验证规则 :

修改\_app/Http/Controllers/Auth/RegisterController.php中的\_validator方法 , 并添加验证信息 :

```
    'captcha' => 'required|captcha',
], [
    'captcha.required' => '验证码不能为空',
    'captcha.captcha' => '请输入正确的验证码',
]);
```

> 安装Laravel-lang , 之前忘记安装了 .

#### User表相关整理

移动user模型文件 , 修改相关依赖 . User相关控制器 . 以及个人页面相关页面等 .

```
# 模型移动修改的文件
app/Models/User.php
config/auth.php
config/services.php
database/factories/UserFactory.php
# 添加资源路由,only限制
Route::resource('users', 'UsersController', ['only' => ['show', 'update', 'edit']]);
# 创建控制器,添加上面的三个方法
php artisan make:controller UsersController
# 创建视图页面
resources/views/users/show.blade.php
# 创建用户编辑页面
resources/views/users/show.edit.php
# 创建用户添加字段的数据迁移,这里暂时先添加avatar头像和introduction描述两个字段
php artisan make:migration add_avatar_and_introduction_and_gender_to_users_table --table=users
$table->string('avatar')->nullable(); # 表示字符串,可以为空
$table->dropColumn('avatar'); # 表示删除列
# 这里添加了一个枚举类型
$table->enum('gender', ['male', 'female', 'unselected'])->default('unselected');
# 给所有字段添加上描述,以免遗忘
->comment('');
php artisan migrate # 执行迁移
# 编辑页面基本配置完成,引入update()方法,这里用Laravel的表单请求验证也就是Request
php artisan make:request UserRequest
会生成一个UserRequest文件.
暂时把权限验证设置为ture.
然后添加规则
return [
    'name' => 'required|between:3,25|regex:/^[A-Za-z0-9\-\_]+$/|unique:users,name,' . Auth::id(),
    'email' => 'required|email',
    'introduction' => 'max:80',
];
再复写一个messages方法,用来自定义一些错误信息.
错误信息提示的模板整理为公共的模板.
然后更新控制器的update方法,跳转到show页面,with跟随一个提交后状态的session闪存信息.Session::exists('danger')
定义闪存信息公共模板lang/zh-CN/message.blade.php管理信息.
访问文本在模板中使用@lang().
# 提交信息别忘了新添加的字段,要添加到fillable中
# show页面的展示修改一下,时间显示用友好的方式diffForHumans()
# 在服务提供者boot时,把管理时间的类Carbon改为中文:Carbon::setLocale('zh');
# commit一下
```

```
# 个人页面的头像等部分使用新路由
# 文件存储系统接入
创建App/Handlers/ImageUploadHandler.php文件
编辑逻辑.添加helper方法排除in_array使用
创建local驱动时,文件保存路径的连接php artisan storage:link
public/storage => storage/app/public
这里本地有问题,暂时使用$file->move移动文件到public下
# 接入七牛驱动,这里部署时暂时使用镜像回源,所有文件依然上传本地 --------------->todo
# 请求验证创建了UserSubjectRequest
# 对图片进行裁剪,使用intervention/image扩展,缓存部分还未实施----------------->todo
# 图片裁剪驱动转换为imagick改用fit以中心裁剪图片
# 添加jq_function.js文件,保存jQ函数,引入app主文件
# 添加头像预览jq函数,调整原来改变文件名函数到pre_img中
# 注意jquery函数调用以及写法,常规function定义函数无法调用
```

```
# User控制器添加中间件限制游客访问
# $this->middleware('auth', ['except' => ['show']]);
# 添加授权认证UserPolicy
php artisan make:policy UserPolicy
判断当前和授权的用户id是否相等
return $currentUser->id === $user->id;
# 注册在认证服务中AuthServiceProvider
# 在控制器中添加拦截,目前直接拒绝访问419,之后改为跳转或者403------------------------------------->todo
$this->authorize('update', $user);
# 修改更新后跳转back()地址
# 修改编辑资料左侧链接
# 更新jq_function格式验证---------------------->todo:没有提示,只是不加载不是图片的文件

# 生成假数据,编辑database/factories/UserFactory.php,填充各个字段生成数据,头像暂用gravatar默认头像
# 生成php artisan make:seed UsersTableSeeder
# 编辑database/seeds/UsersTableSeeder.php,index[0]默认分配给管理员用户
# 然后在DatabaseSeeder中call一下.
# php artisan migrate:refresh --seed 生成数据

# 配置默认头像,增加Observers,注册用户时,自动添加默认头像,注册到boot中,和时间按个一样
# 这里头像前端展示去掉asset()的包裹,后期配置使用配置url,现在在imageHandler中设置url地址----------------->todo
# 添加sudo库切换用户,注册为只在本地显示.
```




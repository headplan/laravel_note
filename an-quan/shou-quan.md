# 授权

### 知识点整理

```
1.授权实现的两种方式
    Gates和Policies
    Gates提供了闭包方式的授权.
    Policies提供了和控制器一样对模型或资源的复杂逻辑的授权

2.Gates
    通常使用Gate门面定义在App\Providers\AuthServiceProvider类中.
    第一个参数:用户实例
    额外参数:其他相关Eloquent模型实例等
    授权动作方法:allows,denies,这两个方法可以不用传用户实例
    allows
    if (Gate::allows('update-post', $post)) {
        // 当前用户可以更新文章...
    }
    denies
    if (Gate::denies('update-post', $post)) {
        // 当前用户不能更新文章...
    }
    forUser方法,判断指定用户(非当前用户)
    if (Gate::forUser($user)->allows('update-post', $post)) {
        // 用户可以更新文章...
    }

3.创建策略类Policies
    生成策略类
    例如:Post模型和与之对应的PostPolicy来授权用户创建或更新的动作.
    php artisan make:policy PostPolicy
    指定相应模型
    php artisan make:policy PostPolicy --model=Post
    所有策略类都通过服务容器进行解析,以便在策略类的构造函数中通过类型提示进行依赖注入.
    
    注册策略类
    使用AuthServiceProvider包含的policies属性完成映射
    protected $policies = [
        Post::class => PostPolicy::class,
    ];

4.编写策略类
    策略方法,例如:
        public function update(User $user, Post $post)
        {
            return $user->id === $post->user_id;
        }
    不带模型的方法:仅检查当前用户是否有创建权限
    public function create(User $user)
    {
        //
    }
    策略过滤器
    定义before方法,在所有策略方法之前执行.
    public function before($user, $ability)
    {
        if ($user->isSuperAdmin()) {
            return true;
        }
    }

5.使用策略授权动作
    通过User模型
        User模型提供了两个方法用于授权动作:can和cant
        if ($user->can('update', $post)) {
            //
        }
        两个参数,第一个参数授权动作,第二个是授权模型
        也可以不依赖模型,直接操作,传递个类名即可,因为create这种,不需要依赖模型
        $user->can('create', Post::clas
       
    通过中间件
    Illuminate\Auth\Middleware\Authorize,别名是can
    Route::put('/post/{post}', function (Post $post) {
    // The current user may update the post...
    })->middleware('can:update,post');
    can的参数第一个是授权动作名,第二个是路由参数.
    不依赖于模型的动作
    Route::post('/post', function () {
    // The current user may create posts...
    })->middleware('can:create,App\Post');

    通过控制器辅助函数
    $this->authorize('update', $post);
    $this->authorize('create', Post::class);    

    通过Blade模板
    使用 @can 和 @cannot 指令
    @can('update', $post)
        <!-- 当前用户可以更新文章 -->
    @elsecan('create', $post)
        <!-- 当前用户可以创建新文章 -->
    @endcan
    
    @cannot('update', $post)
        <!-- 当前用户不能更新文章 -->
    @elsecannot('create', $post)
        <!-- 当前用户不能创建新文章 -->
    @endcannot
    
    不依赖模型的动作
    @can('create', Post::class)
        <!-- 当前用户可以创建文章 -->
    @endcan
```






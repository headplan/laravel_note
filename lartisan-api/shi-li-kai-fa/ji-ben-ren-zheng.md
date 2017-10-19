# 基本认证

**post请求**

Laravel5.2+以后的新版本支持了中间件分组 , 这里已经不需要去掉CSRF中间件 , 或者说已经分组了web与api两个中间件的分组 .

旧版本可以直接修改VerifyCsrfToken中间件 :

```php
class VerifyCsrfToken extends BaseVerifier
{
    /**
     * The URIs that should be excluded from CSRF verification.
     *
     * @var array
     */
    protected $except = [
        // 这里可以直接添加过滤掉的URL
    ];

    public function handle($request, Closure $next)
    {
        // 如果是来自 api 域名，就跳过检查
        if ($_SERVER['SERVER_NAME'] != config('api.domain'))
        {
            return parent::handle($request, $next);
        }

        return $next($request);
    }
}
```

**HTTP Basic Auth 认证**

Laravel的中间件中已经自带了这种认证方式 , 我们可以在路由中添加middleware或者在控制器中添加 .

这里简单的介绍一下HTTP Basic Auth .

HTTP Basic Auth : HTTP基本认证

维基百科 : [https://zh.wikipedia.org/wiki/HTTP基本认证](https://zh.wikipedia.org/wiki/HTTP基本认证)

* 如它的名字，是基础的验证，所以会比较简单
* 会直接把用户名、密码的信息放在请求的 Header 中
* 携带信息的时候是把用户名、密码简单的拼接 & base64 掉，主要是为了解决账号、密码中可能存在的编码问题
* 额外的好处是不会直接看到账号密码，但是本质还是明文传输账号、密码，所以如果不是 HTTPS 会很容易被监听到
* 每次请求都会携带这些信息，curl 需要加`-u, --user USER[:PASSWORD] Server user and password`来设置，浏览器应该会有个缓存

Laravel中对其的配置在config/auth.php中可以看到 . 里面用到了user表 . 我们先添加一条用户数据 :

```php
php artisan tinker
>>> $data = [
... "name" => "admin",
... "email" => "admin@admin.com",
... "password" => "admin",
... ];
>>> namespace App;
>>> User::create([
... 'name' => $data['name'],
... 'email' => $data['email'],
... 'password' => bcrypt($data['password']),
... ]);
```

控制器添加middleware对方法进行过滤

```php
$this->middleware('auth.basic',['only'=>['store','update']]);
```

简单的在store方法中进行验证 , 也就是post添加数据 . 

Laravel自带的token api认证 , 可以查看TokenGuard类 , 这里不再记录 . 






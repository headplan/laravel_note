# CSRF保护

### 知识点整理

```
1.简介:避免跨站请求伪造
    {{ csrf_field() }}表单生成token
2.从 CSRF 保护中排除指定 URL
    class VerifyCsrfToken
3.X-CSRF-Token
令牌发送方式<meta name="csrf-token" content="{{ csrf_token() }}">
4.X-XSRF-Token
令牌在XSRF-TOKEN的Cookie中
```

跨站请求伪造是一种通过伪装授权用户的请求来利用授信网站的恶意漏洞.

Laravel 自动为每一个被应用管理的有效用户会话生成一个CSRF“令牌”,该令牌用于验证授权用户和发起请求者是否是同一个人.

任何时候在 Laravel 应用中定义HTML表单,都需要在表单中引入CSRF令牌字段,这样CSRF保护中间件才能够正常验证请求.想要生成包含CSRF令牌的隐藏输入字段,可以使用辅助函数`csrf_field`来实现:

```
<form method="POST" action="/profile">
    {{ csrf_field() }}
    ...
</form>
```

中间件组 web 中的中间件`VerifyCsrfToken` 会自动为我们验证请求输入的 `token` 值和 Session 中存储的 `token` 是否一致.

### 从CSRF保护中排除指定URL

有时候我们需要从CSRF保护中排除一些URL.例如,如果要使用Stripe来处理支付并用到他们的webhook系统,这时候就需要从Laravel的CSRF保护中排除webhook处理器路由,因为Stripe并不知道要传什么token值给路由.

通常需要将这种类型的路由放到文件 routes\/web.php 里,中间件组 web 之外.此外,也可以在 `VerifyCsrfToken` 中间件中将要排除的 URL 添加到 `$except` 属性数组:

```
<?php
namespace App\Http\Middleware;
use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as BaseVerifier;
class VerifyCsrfToken extends BaseVerifier
{
    // 从CSRF验证中排除的URL
    protected $except = [
        'stripe/*',
    ];
}
```

### X-CSRF-Token

除了将CSRF令牌作为POST参数进行验证外,还可以通过设置`X-CSRF-Token`请求头来实现验证,`VerifyCsrfToken`中间件会检查`X-CSRF-TOKEN`请求头,首先创建一个meta标签将令牌保存到该meta标签:

```
<meta name="csrf-token" content="{{ csrf_token() }}">
```

然后在js库\(如jQuery\)中添加该令牌到所有请求头,这为基于AJAX的应用提供了简单,方便的方式来避免CSRF攻击:

```
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
```

### X-XSRF-Token

Laravel还会将CSRF令牌保存到名为`XSRF-TOKEN`的Cookie中,可以使用该Cookie值来设置`X-XSRF-TOKEN`请求头.一些JS框架,比如Angular,会自动进行设置,基本不需要手动设置.


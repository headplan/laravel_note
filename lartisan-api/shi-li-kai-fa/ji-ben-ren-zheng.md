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




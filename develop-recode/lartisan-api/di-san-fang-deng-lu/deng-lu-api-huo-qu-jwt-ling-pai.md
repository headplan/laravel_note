# 登录 API 获取 JWT 令牌

JWT是JSON Web Token 的缩写 , 是一个非常轻巧的规范 , 这个规范允许我们使用 JWT 在用户和服务器之间传递安全可靠的信息 .

JWT 由头部（header）、载荷（payload）与签名（signature）组成 . 一个 JWT 类似下面这样 :

```js
{
    "typ":"JWT",
    "alg":"HS256"
}
{
    "iss":"http://larabbs.test",
    "iat":1515733500,
    "exp":1515737100,
    "nbf":1515733500,
    "jti":"c3U4VevxG2ZA1qhT",
    "sub":1,
    "prv":"23bd5c8949f600adb39e701c400872db7a5976f7"
}
signature
```

* 头部申明了加密算法 ; 
* 载荷中有两个比较重要的数据 , 
  * `exp`是过期时间
  * `sub`是 JWT 的主体 , 这里就是用户的 id
* 最后的 signature 是由服务器进行的签名 , 保证了 token 不被篡改

> JWT 最后是通过 Base64 编码的 , 也就是说它可以被翻译回原来的样子来的 . 所以不要在 JWT 中存放一些敏感信息 .

用户 id , 过期时间等数据都保存在 Token \(即JWT\) 中了 , 所以并不需要将 Token 保存在服务器中 , 客户端请求的时候在 Header 中携带 Token , 服务器获取 Token后 , 进行`base64_decode`即可获取数据进行校验 , 由于已经有了签名 , 所以不用担心数据被篡改 .

#### Token 验证

有了 token 之后该如何验证 token 的有效性 , 并得到 token 对应的用户呢 ? 其实原理很简单 , DingoApi 为我们准备好了`api.auth`这个中间件 :

* 获取客户端提交的 token
* 检测 token 中的签名 signature 是否正确
* 判断 payload\(载荷\) 数据中的 exp，是否已经过期
* 根据 payload 数据中的 sub，取数据库中验证用户是否存在
* 上述检测不正确，则抛出相应异常

#### 安装 jwt-auth

```
$ composer require tymon/jwt-auth:1.0.0-rc.2
```

安装完成后需要设置一下 JWT 的 secret , 这个 secret 很重要 , 用于最后的签名 , 更换这个secret 会导致之前生成的所有 token 无效 .

```
php artisan jwt:secret
```

生成后可以查看.env文件最后一行 , `JWT_SECRET`

修改 config/auth.php，将 api guard 的 driver 改为 jwt .

```php
'api' => [
    'driver' => 'jwt',
    'provider' => 'users',
],
```

修改 config/api.php , auth 中增加 JWT 相关的配置

```php
'auth' => [
    'jwt' => 'Dingo\Api\Auth\Provider\JWT',
],
```

user 模型需要继承`Tymon\JWTAuth\Contracts\JWTSubject`接口 , 并实现接口的两个方法 getJWTIdentifier\(\) , getJWTCustomClaims\(\)

```php
class User extends Authenticatable implements JWTSubject
{
    #...

    /**
     * 返回了 User 的 id
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * 额外在JWT载荷中增加的自定义内容
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }

}
```

在tinker中测试一下 , 生成一个token . 

```php
$user = App\Models\User::first();
Auth::guard('api')->fromUser($user);
```




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

生成base64的token . 可以解码 , 就可以看到一个jwt .

jwt-auth 有两个重要的参数 , 可以在 .env 中进行设置

* JWT\_TTL 生成的 token 在多少分钟后过期 , 默认 60 分钟
* JWT\_REFRESH\_TTL 生成的 token , 在多少分钟内 , 可以刷新获取一个新 token , 默认 20160 分钟 , 14天 . 

这里需要理解一下 JWT 的过期和刷新机制 , 过期很好理解 , 超过了这个时间 , token 就无效了 . 刷新时间一般比过期时间长 , 只要在这个刷新时间内 , 即使 token 过期了 , 依然可以换取一个新的 token , 已达到应用长期可用 , 不需要重新登录的目的 .

---

#### 用户登录

**添加路由**

```php
# 登录
$api->post('authorizations', 'AuthorizationsController@store')
    ->name('api.authorizations.store');
```

**创建登录的 request**

```
php artisan make:request Api/AuthorizationRequest
```

```php
public function rules()
{
    return [
        'username' => 'required|string',
        'password' => 'required|string|min:6',
    ];
}
```

**修改控制器**

```php
public function store(AuthorizationRequest $request)
{
    $username = $request->username;

    # 指定过滤器过滤,判断用户名是邮箱还是手机号
    filter_var($username, FILTER_VALIDATE_EMAIL) ? $credentials['email'] = $username : $credentials['phone'] = $username;
    $credentials['password'] = $request->password;

    # 生成token
    if (!$token = \Auth::guard('api')->attempt($credentials)) {
        return $this->response->errorUnauthorized('用户名或密码错误');
    }

    # 返回数据
    return $this->response->array([
        'access_token' => $token,
        'token_type' => 'Bearer',
        'expires_in' => \Auth::guard('api')->factory()->getTTL() * 60
    ])->setStatusCode(201);
}
```

用户可以使用邮箱或者手机号登录 , 最后返回 token 信息及过期时间`expires_in` , 单位是秒 , 这里返回的结构很像 OAuth 2.0 , 使用方法也与 OAuth 2.0 相似 .

使用postman测试 .

**继续修改控制器**

之前第三方登录只返回了userid .  提取公共部分 , 第三方登录获取 user 后 , 可以使用 fromUser 方法为某一个用户模型生成token .

```php
protected function respondWithToken($token)
{
    return $this->response->array([
        'access_token' => $token,
        'token_type' => 'Bearer',
        'expires_in' => \Auth::guard('api')->factory()->getTTL() * 60
    ]);
}
```

```php
$token = Auth::guard('api')->fromUser($user);
return $this->respondWithToken($token)->setStatusCode(201);
```

---

#### 刷新/删除 token

任何一个永久有效的 token 都是相当危险的 , 通过任意方式泄露了 token 之后 , 用户的相关信息都有可能被利用 . 所以为了安全考虑 , 任何一种令牌的机制 , 都会有过期时间 , 过期时间一般也不会太长 . 那么 token 过期以后 , 难道要用户重新登录吗 ? 像 OAuth 2.0 有`refresh_token`可以用来刷新一个过期的`access_token`, jwt-auth 同样也为我们提供了刷新的机制 , 只要在可刷新的时间范围内 , 即使 token 过期了 , 依然可以调用接口 , 换取一个新的token . 这对于 APP 长期保持用户登录状态是十分重要的 .

**添加路由**

```php
# 刷新token
$api->put('authorizations/current', 'AuthorizationsController@update')
    ->name('api.authorizations.update');
# 删除token
$api->delete('authorizations/current', 'AuthorizationsController@destroy')
    ->name('api.authorizations.destroy');
```

**编辑控制器**

```php
public function update()
{
    $token = Auth::guard('api')->refresh();
    return $this->respondWithToken($token);
}

public function destroy()
{
    Auth::guard('api')->logout();
    return $this->response->noContent();
}
```

两个方法我们都需要提交当前的 token , 格式为

```
Authorization: Bearer {token}
```

**使用postman测试接口**

删除 token 的场景就是用户退出 APP , 将当前的 token 禁用掉 . 注意删除使用的 HTTP 方法是 DELETE , 返回的状态码是 204 , 因为对于删除这类的事件 , 只需要告诉客户端成功了 , 没什么需要返回的信息 . 




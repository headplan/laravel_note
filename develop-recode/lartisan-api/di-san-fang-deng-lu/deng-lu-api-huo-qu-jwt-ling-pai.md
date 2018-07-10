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




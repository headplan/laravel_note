# HTTP\_Session

HTTP协议是建立在TCP/IP协议上的一种无状态的请求/响应协议 , 当客户端向服务端发送一次请求时 , 首先是建立TCP连接 , 接着客户端向服务器发送请求 , 服务器处理完请求后会断开TCP连接 , 而在处理完客户端的请求后服务器不会存储客户端的任何信息 , 也就是说正常情况下 , 不同的客户端发送的请求对于服务器来说是一样的 , 独立的 . 所以 , 由于无法掌握客户端的信息 , 服务器就很难对不同客户做出不同的响应 , 例如权限控制 , 历史记录等 . 早起可能会使用承载用户信息的HTTP头信息或者跟踪客户端IP地址等方式识别多用户 , 但都有缺陷 , 网景公司开发的Cookie技术以及他的扩展Session等技术都很好的解决了识别用户, 实现会话控制 .

#### **Cookie**

Cookie一般有会话和持久之分 . 会话一般用于临时应用 , 当用户退出浏览器即删除 ; 持久Cookie生存时间更长 , 退出浏览器或重启计算机依然存在 , 一般通过Expires或Max-Age参数进行特殊设置 .

Cookie相当于服务器端给客户端发送的用于给每个客户端进行标记的标识 , 而客户端在接收服务器添加标识的响应头时会记录这个标识并在访问某个范围的服务器时携带这个标识 , 也就是客户端再次访问服务端时 , 需要在请求头中附加这个标识 , 这样服务端就能对客户端进行跟踪了 .

服务器通过Set-Cookie或Set-Cookie2响应头部将其发送给客户端 , 而客户端是通过Cookie请求头发送给服务端的 . 在PHP中 , 服务端通过setcookie\(\)函数设置Cookie的响应头 , 通过$\_COOKIE全局数组来获取客户端发送的Cookie信息 .

#### **Laravel中的Cookie**

**附加Cookie至响应**

通过响应对象的`cookie`方法可以生成`cookie`并附加至响应实例 :

```
return response($content)
                ->header('Content-Type', $type)
                ->cookie('name', 'value', $minutes);
```

`cookie`方法也接受另外几个参数 , 参数和给予原生 PHP 的[setcookie](https://secure.php.net/manual/en/function.setcookie.php)方法的参数有着相同的目的和含义

```
->cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)
```

**生成 Cookie 实例**

也可以先生成cookie实例 , 在一段时间以后再生成一个可以给定`Symfony\Component\HttpFoundation\Cookie`的响应实例

```
$cookie = cookie('name', 'value', $minutes);

return response('Hello World')->cookie($cookie);
```

**Cookie门面**

这里不需要返回响应即可

```
Cookie::queue('test', 'test');
```

**Cookie加密**

Laravel 生成的所有 cookie 都是加密并通过签名验证的 , 因此他们并不能够在客户端被修改和读取 . 如果要对部分 cookie 禁用加密 , 可以使用`App\Http\Middleware\EncryptCookies`中间件的`$except`属性 :

```
protected $except = [
    'cookie_name',
];
```

> 生成不加密的Cookie , 为了和JS交互 , 还需要设置http only为false

```
Cookie::queue('cookie_for_js', 'Hello', $minutes = 99999999, $path = null, $domain = null, $secure = false, $httpOnly = false);
```

**从请求中获取 Cookie 值**

使用`Illuminate\Http\Request`实例中的`cookie`方法即可 :

```
$value = $request->cookie('name');
```

---

#### Session

Session相当于Cookie的升级版 , Cookie的工作机制是将信息记录在客户端 , Session则是将信息记录在服务器端 , 服务器存储信息的方式可以是文件 , 数据库 , 内存等 .

以文件存储方式为例 :

1. 客户端第一次访问某服务区
2. 服务器通过Cookie发送sessionID给客户端 , 并在服务器建立一个与sessionID同名的文件用于存储信息 , 而sessionID不能重复 , 即不同客户端的sessionID是不同的
3. 客户端再次访问服务器时会携带服务器发送给客户端的sessionID
4. 服务器根绝客户端发送的sessionID查找对应的文件 , 读取文件中的内容

上面的步骤可以看出 , session依赖Cookie , 当然不用Cookie也可以依赖URL .

开启PHP自带的session功能 , 在需要共享客户端信息的脚本中通过session\_start\(\)函数开启 , 然后就可以向$\_SESSION全局数组中存入或读取数据 , 而$\_SESSION数组与其他数组不同的是 , 当向该数组中添加数组时 , PHP还会将其中的数据序列化写入session文件中 , 每次开启session时 , PHP会将session文件中的数据读取到该全局数组中 , 实现数据共享的功能 . 

**session\_start\(\)函数** - 生成一个sessionID添加到响应头 , 生成一个同名的文件存储在服务器中 , 同时也会判断客户端是否发送sessionID , 如果发送 , 则不再向客户端发送sessionID , 同时查找sessionID同名的文件 , 并将存储的数据读取到$\_SESSION数组中

**$\_SESSION\['name'\] **- 使用全局数组存储添加内容 , PHP会将这个数组中的内容存储到文件中 . 

Session中数据的删除相对于Cookie略微繁琐一些 : 

* 开启session - session\_start\(\)
* 清空session数组 - unset或者赋值\[\]
* 删除客户端Cookie中存储的sessionID - setCookie\(session\_name\(\),"",time\(\)-10,"/"\) 设置为过期
* 删除服务器中的session文件 - session\_destroy\(\)

这里简单描述了session的工作机制 , 详细内容查看php\_note中的记录 , 和php.ini中的关于session的配置

#### Laravel框架中的session机制






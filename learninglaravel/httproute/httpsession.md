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

Laravel重新设计了session的处理机制 . 同时Laravel附带支持了多种Session后端驱动 , 它们都可以通过语义化统一的API访问 , Memcached , Redis , 数据库等 . \(这里依然使用Cookie作为session的驱动 , 即通过Cookie传输sessionID\) .

* session的启动
  * 客户端访问服务器
  * 服务器开启session
  * 检测请求中Cookie是否携带sessionID
  * 携带则使用该ID , 没有则生成新的sessionID
* session的操作
  * 根绝sessionID恢复之前存储的数据
  * 请求处理期间可以使用恢复的数据
  * 也可以向session中继续添加或删除数据
* session的关闭 - 返回响应时 , 将session中的数据存储到相应的位置 , 以备下一次请求到来时使用并发送sessionID的Cookie

**session的启动**

请求首先经过中间件 , session的启动也是在开启会话中间件中handle\(\)函数开启并完成的 .

```php
\Illuminate\Session\Middleware\StartSession::class
public function handle($request, Closure $next)
{
    $this->sessionHandled = true;
    if ($this->sessionConfigured()) {
        $request->setLaravelSession(
            $session = $this->startSession($request)
        );

        $this->collectGarbage($session);
    }

    $response = $next($request);

    if ($this->sessionConfigured()) {
        $this->storeCurrentUrl($request, $session);

        $this->addCookieToResponse($response, $session);
    }

    return $response;
}
```

一个以$response = $next\($request\);分割 , 之前的代码为session开启阶段 , 之后的为session关闭阶段 .

**开启阶段**

$this-&gt;sessionConfigured\(\)检测session驱动的配置\(driver\) , 如果配置了则通过startSession\(\)函数开启session , 并在请求中添加session实例对象 . Larave是通过一个类的实例来管理session的内容的 . session的开启阶段可以分为四步 :

* 检测配置 - 配置文件在config/session.php中完成 , 主要配置session驱动 , 生存时间 , 是否加密和Cookie名称等
  * 过程中用到Session管理器\(SessionManager类实例\) , 在session中间件初始化\(\_\_construct\)的过程中通过依赖注入生成 , 在服务容器中注册的别名为session , 在SessionServiceProvider中的registerSessionManager\(\)中实现 . 完成了依赖注入 , 即有了session管理器 , 接下来就是验证session的驱动了 , 通过管理器实例获取session的配置信息 , 检测driver项 , 即sessionConfigured\(\)中的判断 , 存在则进入下一步 . 
* session实例化 - Session的开启其实就是完成其实例化的过程 , Larave中session的实例有加密和非加密之分 , 也就是EncryptedStore类或Store类的实例 , 主要区别也是数据存储和读取的过程中对数据的加密和解密 . 其实EncryptedStore类继承了Store类 . 
  * Store类初始化时需要hander驱动 , 符合SessionHandlerInterface接口 , 接口定义了7个函数接口 , 即开启,关闭,读取,写入,销毁,回收 , 而驱动的形式可能不同 , 文件驱动 , 数据库驱动Memcache等等 , 区别就是存储媒介不同 , 满足驱动接口即可 . 
    * [http://php.net/manual/zh/class.sessionhandlerinterface.php](http://php.net/manual/zh/class.sessionhandlerinterface.php)
  * Session类的实例化以及配置是通过$this-&gt;getSession\($request\)实现的 , 分为三步 : 
    * 首先 , 根据session配置信息通过session管理器获取负责session实例化函数的名称;
    * 调用该实例化函数完成session类的实例化;
    * 完成sessionID的获取;
    * 总结起来就是 , 通过getDefaultDriver\(\)函数获取session默认驱动的配置信息 , 就是配置文件中的driver 即file , 然后拼装实例化函数名$method = createFileDriver , 最后通过这个函数完成Session的实例化 . 
  * 在session实例化的过程中会通过配置信息判断是否加密 , 就是配置中的encrypt . 然后通过buildSession\(\)函数完成Store类的实例化 , 即Laravel的session管理类 . 这个类的初始化需要两个重要的参数 , 一个就是Cookie的名称 , 即配置中的Larave\_session , 另一个就是数据的存储形式 , 默认为File , 所以就是FileSessionHandler , 这两个也是都可以在配置文件中配置的 . 
  * 最后实例化完成 , 获取sessionID , 即检查请求中的Cookie是否有$session-&gt;getName\(\)获取的 . 没有就创建一个 , 通过generateSessionId\(\)重新生成的\(在setId\(\)中\) . 
* 开启session - 通过Store类实例中的start\(\)函数开启 , 其实就是根据sessionID将对应的数据从存储媒介中取出来 , 放到Store的$attributes数组属性中 . 
  * 数据的读取是通过Store实例的readFromHandler完成的 , 其中的$this-&gt;handler-&gt;read\($this-&gt;getId\(\)\)完成了数据的读取 , 这里就用到了前面说的驱动接口 , 接口中完成了read方法 .
* 将session实例传递给请求实例

**启动阶段的配置 , 以及不同存储引擎的配置查看Session配置中的内容**

**session的操作**

Session开启的最终结果是生成一个session实例\(也就是Store实例\) , 在程序处理请求生成响应的过程中 , 就可以通过从操作这个实例实现对session的操作 .

1.获取实例

获取session实例和获取请求实例基本相同 . 三种方式 :

* 通过请求获取 - $request-&gt;session\(\) , session启动的最后一步就是将session实例传递给请求 . 
* 通过Facade外观调用 - session::put\('name', 'kony.k'\) , 其流程是
  * 通过session的Facade类的getFacadeAccessor\(\)方法返回服务名称"session" , 其对应的实例对象是SessionManager类实例 , 所以会调用该类的put\(\)方法 , 但该类没有定义这个方法 , 所以调用其继承的Manager类中的\_\_call\(\)魔术方法 , 于是调用了$this-&gt;driver\(\)的put方法 , 而$this-&gt;driver\(\)返回的正式记录的session实例即Store类实例 , 也会调用Store类实例的方法 . 
* 全局辅助函数 - session\(\)-&gt;driver\(\);
  * session\(\)辅助函数返回名称为session的服务 , 即SessionManager类实例 , 然后通过driver\(\)方法获取当前配置的session实例 

2.操作实例

* 数据存储
  * Session::put\('name','kony.k'\); - 将数据存储到Store类实例的$attributes属性中 , 即$attributes\['name'\] = 'kony.k';
    * $request-&gt;session\(\)-&gt;put\('key', 'value'\);
    * session\(\)-&gt;put\('key','value'\);
    * session\(\['key' =&gt; 'value'\]\);
  * Session::push\('name.first','123'\); - 以数组的形式存储到$attributes属性中 , 即$attributes\['name'\]\[''first'\] = '123';
    * $request-&gt;session\(\)-&gt;push\('user.teams', 'developers'\);
  * 两个方法接收参数形式是一样的 , 也可以只是一个数组参数 , 这里只有session\(\[\]\)的形式必须接收数组参数才是存储的意思 , 其他的无所谓 . push\(\)函数调用了put函数 , 还有就是字面的意思 , push不会覆盖 , put会覆盖同key的session .  
* 数据读取
  * 读取和存储的方法是对应的 , 对应的有get\(\)和pull\(\)方法 , 参数为key , 第二个参数可以设置如果没有获取到时返回的默认值 . 读取参数key支持name.pic的方式获取数组中值 . pull\(\)方法会在取回值时进行删除 . 
  * 还有几个相关的方法 , 如 : 
    * all\(\)方法获取所有session数据
    * exists\(\)方法检查session值是否存在 , null也表示存在 , 返回true . \(仅支持一维数组\)
    * has\(\)方法同样是检查值是否存在 , null表示不存在 , 如果该值存在并且不为null , 那么则返回 true . \(仅支持一维数组\)
* 数据删除
  * Session::forget\('key'\) - 从 Session 内删除一条数据
  * Session::flush\(\) - 删除 Session 内所有数据
* 数据暂存
  * Session::flash\('key', 'value'\) - 将session数据保留到下一次 HTTP 请求 , 两个注意参数必填 , 第一个参数为字符串 . 
  * Session::reflash\(\) - 留闪存数据给更多请求 . 将数据刷新一次 , 使得本次应该删除的数据不再删除 . 
  * Session::keep\(\['a','b'\]\) - 刷新指定数据 . 如果将原session的key也设置为暂存数据 , 再次请求也会消失 . 
  * 其实在session中 , 有一个\_flash键 , 专门用来存储暂存数据 , 而该键值下又有两个键 , 分别为old和new , 用来记录暂存数据的新旧 , 即本次请求存储的暂存数据为新\(new\) , 上次存储的暂存数据为旧\(old\) , 当本次请求结束时 , 会将旧的数据删除 , 而新的数据保留 , 到下一次请求时 , 上次新的就变成旧的了 , 以此循环 , 函数reflash和keep就是old中准备删除的 , 重新放到新的里 . 
* 重新生成SessionID - $request-&gt;session\(\)-&gt;regenerate\(\);
  * 在LoginController控制器中的trait有对其的应用 , 重新生成 Session ID , 通常是为了防止恶意用户利用 session fixation 对应用进行攻击 . 

**Session的关闭\(就是到最后的响应\)**

关闭可分为四个步骤 , 存储当前的URL , 垃圾回收 , 添加响应首部和存储session数据 . 一切的一切依然都发生在session中间件handle函数中 . 

当前URL的存储需要满足请求方法为get , 具有路由并且非ajax的请求 . 这发生在$this-&gt;storeCurrentUrl\($request, $session\);中 . 

垃圾回收是通过$this-&gt;configHitsLotter\($config\)进行判断的 , 其实就是配置文件中的lottery进行配置的 , 它是一个数组 , 第一个值是阈值 , 第二个是产生的随机数的最大值 . 默认为\[2,100\] , 意思就是随机生成1-100的数 , 如果生成的随机数小于2 , 就触发垃圾回收 , 也就是有1%的几率触发 , 回收过期的session .  

然后是添加Cookie到响应头 . 过程是先判断session是否是持久化存储\(不是数组驱动的就是持久化的\) , 持久化的情况下会将SessionID设置到响应头 , 就是在响应实例中的$headers属性中添加一个Cookie实例 , 用于发送SessionID . 






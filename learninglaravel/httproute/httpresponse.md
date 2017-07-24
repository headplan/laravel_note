# HTTP\_Response

对于请求的实例化 , 可以将其看做是HTTP请求参数封装的过程 , 包括请求报文的请求行 , 头部字段和主体三部分 , 而对于HTTP响应实例的生成 , 也可以看做是对响应参数的封装过程 , 包括响应报文的起始行 , 头部字段和主体三部分 , 最终生成的响应实例对象常用属性以及存储内容 :

* $version - 协议版本
* $statusCode - 响应状态码
* $statusText - 响应原因短语
* $headers - 响应头部字段
* $content - 响应主体

Laravel框架响应生成的三种形式 :

* 只生成响应的主体内容部分 ; 
* 生成响应的头部和主体部分 ; 
* 生成重定向的响应 , 即只包含响应的重定向头部 . 

其他根据请求完成 , 例如起始行就是响应发送时 , sendHeaders\(\)函数中的header\(...\)发送的 .

#### 生成响应的主体内容

响应的主题内容可以简单的看做是一个很长的字符串 , 响应主体内容部分是只用了`echo $this->content`发送 , 所以可以直接返回字符串 , 当然也可以返回视图文件\(也相当于字符串\) .

* 字符串
* 数组\(自动将数组转为JSON响应\)
  * Eloquent集合也可以返回自动转为JSON响应

返回的主体会被Response实例保存在$content属性中 , hearder头会通过调用Routing\Router类的prepareResponse\(\)方法生成 .

#### 生成自定义响应的实例

主体部分直接return字符串 , 其他不会Larave框架会自动生成 , 当然自动生成的部分也可以自定义 . 一般我们不会返回字符串或者数组 , 而是返回整个Response实例或者视图 .

Illuminate\Http\Response实例继承自Symfony\Component\HttpFoundation\Response类 , 构造函数调用的是Symfony的Response类的构造函数 , 自定义响应的 HTTP 状态码和响应头信息的方法是Laravel的响应类中的header方法 .

生成响应实例 :

* 直接new Response类
* 通过response\(\)全局函数

他们的参数是$content,$status,$headers , 如果包含参数 , 则直接返回生成的实例 , 即

```
return $factory->make($content, $status, $headers);
```

如果不包含参数则返回生成响应的实例$factory , 然后通过header\(\)等方法添加头信息等 , 这里只是用法不同而已 .

这些方法都在ResponseTrait中 .

* header\(\)
* withHeaders\(\[\]\)

**代码示例查看相关分支**

#### 生成重定向的响应

重定向响应实际上是`Illuminate\Http\RedirectResponse`类的实例 , 而该类继承了`Symfony\Component\HttpFoundation\RedirectResponse`类的实例 , 这个类又继承了`Symfony\Component\HttpFoundation\Response`类 , 因此可以将重定向响应看做是一个特殊的响应 , 只是在响应的header中包含了Location重定向字段 . Larave的RedirectResponse类在Symfony的基础上封装了跟多的函数 , 例如一次性session , 自定义header等 .

* **redirect\(\)** - 重定向指定路由
* **redirect\(\)-&gt;route\('login'\)** - 重定向至**命名\(有name的;\)**路由
* **redirect\(\)-&gt;route\('login', \['id' =&gt; 1\]\)** - 传递参数 , 跟着路由传到重定向到url上
  * 如果路由原来有参数直接传递 - /login/{id}
  * 如果路由没有参数 - /login?id=
* **redirect\(\)-&gt;route\('profile', \[$user\]\)** - 传递模型本身即可 , ID会自动提取
  * 这里文档写的有些模糊 , 其实用到了路由模型绑定 , **查看代码实例**\(直接隐式绑定了 , 也可以在提供者中绑定\)
  * 要更改自动提取的路由参数的键值 , 可以重写Eloquent 模型里的`getRouteKey`方法
* **redirect\(\)-&gt;action\('HomeController@index', \['id' =&gt; 1\]\)** - 重定向至控制器行为 , 第二个参数是给控制器传参的
* **redirect\('dashboard'\)-&gt;with\('status', 'Profile updated!'\)** - 重定向并附加Session闪存数据
* **back\(\)-&gt;withInput\(\)** - 重定向至上级一页面
  * 由于此功能利用了Session , 请确保调用back函数的路由是使用web中间件组或应用了所有的Session中间件
  * 查看Auth示例

重定向响应实例的生成可以分为两步 , 第一是实现重定向生成器的实例化 , 即app\('redirect'\)的实现过程 , 通过容器解析服务 , 注册进路由服务提供者中 , redirect实例还包括了url生成器实例也在路由服务提供者中 , 第一步完成后第二部就是通过生成器生成这个重定向实例 . 其过程是 , redirect\(\)生成一个重定向生成器 , 该类完成获取以及重定向实例的生成 . \(生成器类Redirector也继承Symfony框架\) . 

Laravel在Symfony的基础上添加了一次性数据存储的功能 , 在Illuminate\Http\RedirectResponse类 , 前面提到的

* with\(\)
* withInput\(\)
* onlyInput\(\)
* withErrors\(\)

还可以通过Redirector类实例的back\(\),route\(\),action\(\)等方法生成请求的URL , 并生成重定向响应的实例 .  

#### 其他响应类型

> 使用全局辅助函数`response`可以轻松的生成其他类型的响应实例 . 当不带任何参数调用`response`时 , 将会返回 Illuminate\Contracts\Routing\ResponseFactory Contract的实现 . Contract 包含许多有用的用来辅助生成响应的方法 .

* 视图响应 - view\(\)函数和全局的view\(\)辅助函数一样 , 参数分别是模板名 , 数据 , 最后一次参数是状态码
  * 也可以定义响应的头信息 , 使用header\(\)函数
* JSON响应 - `Content-Type`响应头信息设置为`application/json`参数接收的数组会自动转为json串
  * 使用`json`方法并结合 withCallback 函数 , 可以创建JSONP响应

#### 响应宏

文档最后举例了响应宏 , 也是通过`macro`方法实现 , 代码中没有示例 , 这里仅copy作为参考 , 具体内容查看相关专题整理

```
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Response;

class ResponseMacroServiceProvider extends ServiceProvider
{
    /**
     * 注册应用的响应宏
     *
     * @return void
     */
    public function boot()
    {
        Response::macro('caps', function ($value) {
            return Response::make(strtoupper($value));
        });
    }
}
```




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








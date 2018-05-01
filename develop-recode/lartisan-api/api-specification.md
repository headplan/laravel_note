# RESTful设计原则

#### HTTPS

HTTPS 为接口的安全提供了保障 , 可以有效防止通信被窃听和篡改 . 部署可以使用cerbot , 在Linux Note中有记录 .

> **另外注意**
>
> 非 HTTPS 的 API 调用 , 不要重定向到 HTTPS , 而要直接返回调用错误以禁止不安全的调用 .
>
> ```
> https://certbot.eff.org/
> ```

#### 域名

将 API 与其主域名区分 , 例 :

```
https://api.test.com
```

或

```
https://www.test.com/api
```

#### 版本控制

通常有两种做法 :

直接写版本号 :

```
https://api.test.com/v1
https://api.test.com/v2
```

使用HTTP请求头里的Accept字段进行区分

```
https://api.test.com/
    Accept: application/prs.test.v1+json
    Accept: application/prs.test.v2+json
```

> Github Api 虽然默认使用了第一种方法 , 但是其实是推荐并实现了第二种方法

#### 用URL定位资源

每一个 URL 都代表着一种资源 , 用名词表示 , 大部分情况下是复数 ,

```
GET /issues                                      列出所有的 issue
GET /orgs/:org/issues                            列出某个项目的 issue
GET /repos/:owner/:repo/issues/:number           获取某个项目的某个 issue
POST /repos/:owner/:repo/issues                  为某个项目创建 issue
PATCH /repos/:owner/:repo/issues/:number         修改某个 issue
PUT /repos/:owner/:repo/issues/:number/lock      锁住某个 issue
DELETE /repos/:owner/:repo/issues/:number/lock   接收某个 issue
```

> 冒号代表变量

一些总结 :

* 资源的设计可以嵌套 , 表明资源与资源之间的关系
* 通常我们访问的是某个资源的集合 , 然后通过id,number或者唯一标识去访问单个资源
* 有些资源也会以单数形式直接出现 , 例如锁资源 , 本身只会有一个 . 

正确的例子 :

```
POST https://api.test.com/topics
GET https://api.test.com/topics/1
POST https://api.test.com/topics/1/comments
DELETE https://api.test.com/topics/1/comments/100
```

#### 用 HTTP 动词描述操作

即 , HTTP 设计的很多动词 , GET/POST等 . 对资源的操作 , 使用这些东西 .

先来看一个概念 :

> 幂等性 :
>
> 指一次和多次请求某一个资源应该具有同样的副作用 , 也就是一次访问与多次访问 , 对这个资源带来的变化是相同的 .

| 动词 | 描述 | 是否幂等 |
| :--- | :--- | :--- |
| GET | 获取资源，单个或多个 | 是 |
| POST | 创建资源 | 否 |
| PUT | 更新资源，客户端提供完整的资源数据 | 是 |
| PATCH | 更新资源，客户端提供部分的资源数据 | 否 |
| DELETE | 删除资源 | 是 |

> 这里可以注意下**PUT和PATCH**两个更新动作 :
>
> PUT 是根据客户端提供了完整的资源数据 , 客户端提交什么就替换为什么 .
>
> PATCH 有可能是根据客户端提供的参数 , 动态的计算出某个值 , 例如每次请求后资源的某个参数减1 , 所以多次调用，资源会有不同的变化 .

还要注意 , GET 请求对于资源来说是安全的 , 通常不允许通过GET改变资源 , 也就是更新或创建 . 在实际应用中 , 为了统计数据方便 , 会有一些例外情况 , 例如帖子详情 , 记录访问次数 , 每调用一次 , 访问次数 +1 等等 .

#### 资源过滤

提供合理的参数供客户端过滤资源 : 

```
?state=closed : 不同状态的资源
?page=2&per_page=100 : 访问第几页数据,每页多少条.
?sortby=name&order=asc : 指定返回结果按照哪个属性排序,以及排序顺序
```

#### 正确使用状态码

正确的使用HTTP提供的丰富的状态码 , 可以让响应数据更具可读性 . 

* 200 OK - 对成功的 GET、PUT、PATCH 或 DELETE 操作进行响应。也可以被用在不创建新资源的 POST 操作上
* 201 Created - 对创建新资源的 POST 操作进行响应。应该带着指向新资源地址的 Location 头
* 202 Accepted - 服务器接受了请求，但是还未处理，响应中应该包含相应的指示信息，告诉客户端该去哪里查询关于本次请求的信息
* 204 No Content - 对不会返回响应体的成功请求进行响应（比如 DELETE 请求）
* 304 Not Modified - HTTP缓存header生效的时候用
* 400 Bad Request - 请求异常，比如请求中的body无法解析
* 401 Unauthorized - 没有进行认证或者认证非法
* 403 Forbidden - 服务器已经理解请求，但是拒绝执行它
* 404 Not Found - 请求一个不存在的资源
* 405 Method Not Allowed - 所请求的 HTTP 方法不允许当前认证用户访问
* 410 Gone - 表示当前请求的资源不再可用。当调用老版本 API 的时候很有用
* 415 Unsupported Media Type - 如果请求中的内容类型是错误的
* 422 Unprocessable Entity - 用来表示校验错误
* 429 Too Many Requests - 由于请求频次达到上限而被拒绝访问

#### 数据响应格式

根据可用性和可读性 , 默认使用 JSON 作为数据响应格式 . 其他响应格式需要在Accept头中指定 , 例如xml : 

```
https://api.test.com/
    Accept: application/prs.test.v1+json
    Accept: application/prs.test.v1+xml
```

对于**错误数据** , 默认使用如下结构 : 

```
'message' => ':message',          // 错误的具体描述
'errors' => ':errors',            // 参数的具体错误描述，422 等状态提供
'code' => ':code',                // 自定义的异常码
'status_code' => ':status_code',  // http状态码
'debug' => ':debug',              // debug 信息，非生产环境提供
```

例如 : 

```
{
    "message": "422 Unprocessable Entity",
    "errors": {
        "name": [
            "姓名 必须为字符串。"
        ]
    },
    "status_code": 422
}

{
    "message": "您无权访问该订单",
    "status_code": 403
}
```




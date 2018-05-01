# RESTful设计原则

#### HTTPS

HTTPS 为接口的安全提供了保障 , 可以有效防止通信被窃听和篡改 . 部署可以使用cerbot , 在Linux Note中有记录 . 

> **另外注意**
>
> 非 HTTPS 的 API 调用 , 不要重定向到 HTTPS , 而要直接返回调用错误以禁止不安全的调用 .

> ```
> https://certbot.eff.org/
> ```




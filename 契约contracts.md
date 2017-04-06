# 契约\(Contracts\)

### 知识点整理

```
1.简介
Laravel 中的契约是指框架提供的一系列定义核心服务的接口.
Illuminate\Contracts\Queue\Queue 契约定义了队列任务需要实现的方法
Illuminate\Contracts\Mail\Mailer 契约定义了发送邮件所需要实现的方法
所有 Laravel 契约都有其对应的GitHub库，这为所有有效的契约提供了快速入门指南，同时也可以作为独立、解耦的包被包开发者使用
2.何时使用契约
松耦合
简单的例子,使用缓存.
原来类中使用了memcache缓存,在初始化中依赖注入了这个实现类,这样耦合太紧了.
契约的方式是,依赖注入缓存契约,然后在别的地方实现这个缓存的方式.
3.如何使用契约
在解析类的构造函数中类型提示这个契约接口.
4.契约列表
https://github.com/illuminate/contracts
```




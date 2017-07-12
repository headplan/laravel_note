# Laravel 5.4

> Laravel 5.4 在 5.3 的基础继续进行优化 : 在邮件和通知中支持Markdown、浏览器自动测试框架Laravel Dusk、Laravel Mix、Blade“组件”和“插槽”、在广播频道上进行路由模型绑定、在集合中支持高阶消息传递、基于对象的Eloquent事件、任务级别的“重试”和“超时”设置、“实时”门面、更好的支持Redis Cluster、自定义透视表（pivot）模型、请求输入清理中间件，等等。此外，官方开发组还review和重构了整个框架的底层代码，以让其更加干净和清晰。
>
> Github上的更新记录\(完整\) :
>
> [https://github.com/laravel/framework/blob/5.4/CHANGELOG-5.4.md](https://github.com/laravel/framework/blob/5.4/CHANGELOG-5.4.md)

#### 使用Markdown编写邮件和通知

该特性允许我们在邮件中利用预置模板和邮件通知组件，由于消息使用Markdown格式编写，因此Laravel可以将这些消息渲染成美观、响应式的HTML模板的同时自动为其生成纯文本副本。例如，下面是一个Markdown格式的邮件示例：

```
@component('mail::message')
# Order Shipped

Your order has been shipped!

@component('mail::button', ['url' => $url])
View Order
@endcomponent

Next Steps:

- Track Your Order On Our Website
- Pre-Sign For Delivery

Thanks,<br>
{{ config('app.name') }}
@endcomponent
```

利用这个简单的Markdown模板，Laravel就可以生成响应式的HTML邮件以及对应的纯文本副本：![](/assets/laravel-email.png)

> 你可以将所有Markdown邮件组件导出到自己的应用中进行自定义。要导出这些组件，可以使用Artisan命令`vendor:publish`来发布标签为`laravel-mail`的资源

### Laravel Dusk {#toc_1}

### Laravel Mix {#toc_2}

### Blade组件&插槽 {#toc_3}

### 广播中的模型绑定 {#toc_4}

### 集合高阶消息传递 {#toc_5}

### 基于对象的Eloquent事件 {#toc_6}

### 任务级的重试&超时 {#toc_7}

### 请求清理中间件 {#toc_8}

### “实时”门面 {#toc_9}

### 自定义透视表模型 {#toc_10}

### 优化Redis集群支持 {#toc_11}

### 迁移默认字符换长度 {#toc_12}




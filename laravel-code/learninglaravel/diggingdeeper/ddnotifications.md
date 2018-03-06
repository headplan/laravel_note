# DD\_Notifications

### Laravel 的消息通知系统

除了发送邮件 , Laravel还支持通过多种频道发送通知 , 包括邮件 , 短信\(例如:Nexmo\)以及Slack . 通知还能存到数据库 , 这样就能在网页界面上显示了 . 通常情况下 , 通知应该是简短 , 有信息量的消息来通知用户你的应用发生了什么 . 例如 , 你在编写一个在线交易应用 , 你应该会通过邮件和短信频道来给用户发送一条「账单已付」的通知 .

#### 创建通知

命令行创建 , 会在`app/Notifications`目录下生成一个新的通知类

```
php artisan make:notification TestNotification
```

生成TestNotification类 , 其中包含了via方法和方法和几个消息构建方法toMail,toArray等 , 针对**指定的渠道**\(via方法\)把通知转**换过为对应**的消息\(toSome\) .

#### 发送通知

通知的发送方法有两种 :

* 通过**Notifiable Trait**中的notify\(\)方法 , 这里的Notifiable Trait也是由两个Trait组成的 , 他们都在Illuminate\Notifications目录下
  * HasDatabaseNotifications Trait
  * RoutesNotifications Trait
  * 可以在任何模型总使用Notifiable Trait , 通过$obj-&gt;notify\(new InvoicePaid\($invoice\)\) , 参数是一个Notification实例 . 
* 通过**Notification Facade**外观 - 主要用在给多个可接收通知的实体发送通知的时候使用
  * Notification::send\($users, new InvoicePaid\($invoice\)\);
  * 第一个参数是可接收通知的实体
  * 第二个参数是Notification实例

再来看看前面创建的**通知类**

`via`方法受到一个`$notifiable`实例 , 它是接收通知的类实例 , 可以用来决定通知用哪个频道来发送 .

```
/**
 * 获取通知发送频道
 *
 * @param  mixed  $notifiable
 * @return array
 */
public function via($notifiable)
{
    return $notifiable->prefers_sms ? ['nexmo'] : ['mail', 'database'];
}
```

开箱即用的通知频道有

* `mail`
* `database`
* `broadcast`
* `nexmo`
* `slack`

更多的频道 , 例如Telegram或Pusher , 可以去[http://laravel-notification-channels.com/看看](http://laravel-notification-channels.com/看看) .

**队列化通知**

在队列化通知前需要配置队列 , 并运行队列处理器 . 发送频道需要调用外部 API 的时候 , 队列化通知会更好加速应用响应 . 在前面的命令行创建通知时 , 其实已经引入了`ShouldQueue`接口和`Queueable`trait

* Illuminate\Bus\Queueable;
* Illuminate\Contracts\Queue\ShouldQueue;

默认ShouldQueue是灰色的 , 如果implements了ShouldQueue接口 , Laravel 会检测并自动将通知的发送放入队列中 , 其他的不用改变 , 如果想延迟发送 , 可以通过`delay`方法来链式操作通知实例

```
$when = Carbon::now()->addMinutes(10);
$user->notify((new InvoicePaid($invoice))->delay($when));
```

---

#### mail




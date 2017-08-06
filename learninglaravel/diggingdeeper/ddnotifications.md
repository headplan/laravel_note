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

* 通过Notifiable Trait中的notify\(\)方法 , 这里的Notifiable Trait也是由两个Trait组成的 , 他们都在Illuminate\Notifications目录下
  * HasDatabaseNotifications Trait
  * RoutesNotifications Trait
* 通过Notification Facade外观






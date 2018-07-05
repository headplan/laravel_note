# 短信提供商

提供商有很多 , 方便测试 , 使用 :

```
https://www.yunpian.com/
```

注册成功后 , 根据相关政策的要求 , 各个短信服务商都会要求我们设置『签名』以及『模板』后 , 才允许发送验证码 , 即使你不使用云片 , 流程也大体相同 .

『签名』- 签名一般是在短信内容开始或者末尾跟的品牌或者应用名称 .

首先填写开发者信息 , 在账号设置中 . 认证成功后即可添加签名信息 . 签名信息需要认证一会 , 同时模板也是需要认证的 , 这里先去添加模板认证 . 默认就是用户收到的短信的模板 . 根据提示添加即可.

等待审核认证 .

#### 安装 easy-sms

easy-sms是一个短信发送组件 , 利用这个组件 , 可以快速的实现短信发送功能 . 查看扩展包与工具中的单独文档 .

#### 调试短信

```
$sms = app('easysms');
try {
    $sms->send(13812345678, [
        'content'  => '【LARTISAN】您的验证码是1234。如非本人操作，请忽略本短信',
    ]);
} catch (\Overtrue\EasySms\Exceptions\NoGatewayAvailableException $exception) {
    $message = $exception->getException('yunpian')->getMessage();
    dd($message);
}
```




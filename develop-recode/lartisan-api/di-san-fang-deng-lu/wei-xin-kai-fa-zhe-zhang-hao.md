# 微信开发者账号

微信不支持个人开发者申请微信登录 , 可以使用测试接口 , 通过网页授权 , 模拟整个流程 .

#### 微信公众平台测试账号

申请公众平台测试账号十分方便 , 直接通过微信登录即可 .

登录后就可以看到看到`appId`和`appsecret` . 然后 ,

需要关注自己的测试公众号 , 只有关注了测试公众号的用户 , 才可以进行授权操作 , 微信扫描`测试号二维码`即可 .

在下面的`体验接口权限表`中我们可以找到`网页授权获取用户基本信息` , 点击最后的修改按钮 . 填入`lartisan.bbs`, 这个是 OAuth 流程中需要提前配置好的回调域名 , 回调地址必须在这个域名下 .

#### 测试 OAuth 流程

因为是公众平台测试账号 , 所以首先需要下载微信web开发者工具 , 方便我们接下来的调试 . 打开工具 , 选择公众号网页项目 .

接下来 , 尝试一下微信网页授权的流程 . 下面这个链接为微信发起 OAuth 的跳转地址 :

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect
```

替换链接中有几个变量 :

* APPID - 测试账号中的`appID` , 填写自己账号的appID
* REDIRECT\_URI - 用户同意授权后的回调地址 , 填写`http://lartisan.bbs`
* SCOPE - 应用授权作用域 , 填写`snsapi_userinfo`
* STATE - 随机参数 , 可以不填 , 保持`STATE`即可 . 

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx070*******5ff&redirect_uri=http://lartisan.bbs&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect
```

在开发者工具中访问上面的链接 , 可以看到微信授权页面 . 点击确认登录就会跳转到成功的跳转回了`REDIRECT_URI`. 注意url中可以看到code参数 . 

请求下面的链接获取token : 

```
https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code
```

需要替换的变量 : 

* APPID 测试账号中的`appID`, 填写自己账号的`appID`
* SECRET 测试账号中的`secret`,填写自己账号的`secret`
* code 上一步获取的 code

使用 PostMan 访问该链接 , 获取到了`access_token`

微信同时返回了`open_id`, 微信`access_token`和`open_id`一起请求用户信息 . 

**通过`access_token`获取个人信息**

```
https://api.weixin.qq.com/sns/userinfo?access_token=ACCESS_TOKEN&openid=OPENID&lang=zh_CN
```

替换链接中的`ACCESS_TOKEN`和`OPENID` , 使用 PostMan 访问 . 

```
{
    "openid": "o*******",
    "nickname": "啊啊啊",
    "sex": 1,
    "language": "zh_CN",
    "city": "昌平",
    "province": "北京",
    "country": "中国",
    "headimgurl": "http://thirdwx.qlogo.cn/mmopen/**********/132",
    "privilege": []
}
```




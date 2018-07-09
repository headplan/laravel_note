# 图片验证码

为了防止攻击者用大量的IP攻击短信接口 , 就可以增加一些成本较高的或者说机器无法识别的图片验证码 .

图片验证码接口流程 :

* 生成图片验证码
* 生成随机的 key , 将验证码文本存入缓存 . 
* 返回随机的 key , 以及验证码图片 . 

#### 安装 gregwar/captcha

不依赖session . 

```
composer require gregwar/captcha
```

#### 开发接口

**新建路由**

```php
# 图片验证码
$api->post('captchas', 'CaptchasController@store')
    ->name('api.captchas.store');
```




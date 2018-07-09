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

**新建控制器和表单验证类**

```
php artisan make:controller Api/v1/CaptchasController
php artisan make:request Api/CaptchaRequest
```

**修改验证类**

```php
<?php

namespace App\Http\Requests\Api;

use Dingo\Api\Http\FormRequest;

class CaptchaRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'phone' => 'required|regex:/^1[34578]\d{9}$/|unique:users',
        ];
    }
}
```

**修改控制器**

```php
<?php

namespace App\Http\Controllers\Api\v1;

use Gregwar\Captcha\CaptchaBuilder;
use App\Http\Controllers\Api\Controller;
use App\Http\Requests\Api\CaptchaRequest;

class CaptchasController extends Controller
{
    public function store(CaptchaRequest $request, CaptchaBuilder $builder)
    {
        $key = 'captcha-' . str_random(15);
        $phone = $request->phone;

        # 生成验证码
        $captcha = $builder->build();
        $expiredAt = now()->addSeconds(120);

        # 存入缓存
        \Cache::put($key, ['phone' => $phone, 'code' => $captcha->getPhrase()], $expiredAt);

        # 返回数据
        $result = [
            'captcha_key' => $key,
            'expired_at' => $expiredAt->toDateTimeString(),
            'captcha_image_content' => $captcha->inline()
        ];

        # 返回201
        return $this->response->array($result)->setStatusCode(201);
    }
}
```

**测试接口**

保存接口道postman .

**集成到短信验证接口**

修改验证

```php
public function rules()
{
    return [
        'captcha_key' => 'required|string',
        'captcha_code' => 'required|string',
    ];
}

public function attributes()
{
    return [
        'captcha_key' => '图片验证码 key',
        'captcha_code' => '图片验证码',
    ];
}
```

修改控制器

主要增加了验证图片验证码的步骤 , 用户手机号从缓存中获取 , 最后清除图片验证码缓存 .

选择测试接口 , 就需要带上参数了 .

```
captcha_key
captcha_code
```

保存接口 . 


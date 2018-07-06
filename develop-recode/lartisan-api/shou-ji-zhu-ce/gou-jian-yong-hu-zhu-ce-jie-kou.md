# 构建用户注册接口

#### 新增路由

```php
# 用户注册
$api->post('users', 'UsersController@store')
    ->name('api.users.store');
```

#### 添加控制器和表单验证类

```
php artisan make:controller Api/v1/UsersController 
php artisan make:request Api/UserRequest
```

**编辑UserRequest的规则**

```php
<?php

namespace App\Http\Requests\Api;

use Illuminate\Foundation\Http\FormRequest;

class UserRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'name' => 'required|string|max:255',
            'password' => 'required|string|min:6',
            'verification_key' => 'required|string',
            'verification_code' => 'required|string',
        ];
    }

    /**
     * 程序验证错误时的消息提示
     * @return array
     */
    public function attributes()
    {
        return [
            'verification_key' => '短信验证码 key',
            'verification_code' => '短信验证码',
        ];
    }
}
```

**编辑控制器逻辑**

```php
<?php

namespace App\Http\Controllers\Api\v1;

use App\Models\User;
use App\Http\Controllers\Api\Controller;
use App\Http\Requests\Api\UserRequest;

class UsersController extends Controller
{
    public function store(UserRequest $request)
    {
        # 获取缓存中的验证码key
        $verifyData = \Cache::get($request->verification_key);

        # 判断验证码不存在说明已经失效了,状态为422
        if (!$verifyData) {
            return $this->response->error('验证码已失效', 422);
        }

        # 判断请求的和缓存中的验证码是否相等,当然不等则返回验证码错误,状态为401
        # 这里的hash_equals是防时序攻击的比较
        if (!hash_equals($verifyData['code'], $request->verification_code)) {
            # 这里默认状态为401
            return $this->response->errorUnauthorized('验证码错误');
        }

        # 创建用户,入库
        $user = User::create([
            'name' => $request->name,
            'phone' => $verifyData['phone'],
            'password' => bcrypt($request->password),
        ]);

        # 清楚验证码缓存
        \Cache::forget($request->verification_key);

        # 创建完成,返回数据,状态201
        return $this->response->created();
    }
}

```

**测试接口**

这里需要先就该模型中的fillablse字段 , 添加phone .


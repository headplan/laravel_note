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




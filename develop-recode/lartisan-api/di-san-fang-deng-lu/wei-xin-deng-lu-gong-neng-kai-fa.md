# 微信登录功能开发

#### 调整用户表结构

首先需要为 users 表增加两个字段 , weixin\_openid , weixin\_unionid , 用来记录微信用户的唯一标识 . 修改 password 字段为 nullable , 因为第三方登录不需要密码 .

```
php artisan make:migration add_weixin_openid_to_users_table --table=users
```

编辑迁移文件 :

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class AddWeixinOpenidToUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('weixin_openid')->unique()->nullable()->after('password');
            $table->string('weixin_unionid')->unique()->nullable()->after('weixin_openid');
            $table->string('password')->nullable()->change();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropColumn('weixin_openid');
            $table->dropColumn('weixin_unionid');
            $table->string('password')->nullable(false)->change();
        });
    }
}
```

运行迁移

```
php artisan migrate
```

#### 路由设计

为了区分用户账号密码登录和第三方登录 , 将登录接口设计为2个 .

**添加第三方登录接口**

```php
# 第三方登录
$api->post('socials/{social_type}/authorizations', 'AuthorizationsController@socialStore')
    ->name('api.socials.authorizations.store');
```

**第三方登录逻辑**

创建 controller 和 request

```
$ php artisan make:controller Api/v1/AuthorizationsController
$ php artisan make:request Api/SocialAuthorizationRequest
```

修改验证规则

```php
public function rules()
{
    $rules = [
        'code' => 'required_without:access_token|string',
        'access_token' => 'required_without:code|string',
    ];

    # 微信还需要验证openid
    if ($this->social_type == 'weixin' && !$this->code) {
        $rules['openid']  = 'required|string';
    }

    return $rules;
}
```

修改模型的fillable字段

```
'weixin_openid', 'weixin_unionid'
```

编写逻辑 :

```php
<?php

namespace App\Http\Controllers\Api\v1;

use App\Http\Requests\Api\SocialAuthorizationRequest;
use App\Http\Controllers\Api\Controller;
use App\Models\User;

class AuthorizationsController extends Controller
{
    public function socialStore($type, SocialAuthorizationRequest $request)
    {
        # 判断是否支持第三方登录类型
        if (!in_array($type, ['weixin'])) {
            return $this->response->errorBadRequest();
        }
        # 设置驱动
        $driver = \Socialite::driver($type);

        try {
            # 如果客户端提交了授权码,就用授权码,要么用access_token
            if ($code = $request->code) {
                $response = $driver->getAccessTokenResponse($code);
                $token = array_get($response, 'access_token');
            } else {
                $token = $request->access_token;
                # 微信还需要openid
                if ($type == 'weixin') {
                    $driver->setOpenId($request->openid);
                }
            }
            # 获取用户数据
            $oauthUser = $driver->userFromToken($token);
        } catch (\Exception $exception) {
            return $this->response->errorUnauthorized('参数错误，未获取用户信息');
        }

        switch ($type) {
            case 'weixin':
                # 判断有没有unionid,只有在用户将公众号绑定到微信开放平台帐号后,才会出现 unionid 字段
                # 微信开放平台只有通过认证才能绑定公众号,这里做了简单的兼容
                $unionid = $oauthUser->offsetExists('unionid') ? $oauthUser->offsetGet('unionid') : null;
                if ($unionid) {
                    $user = User::where('weixin_unionid', $unionid)->first();
                } else {
                    $user = User::where('weixin_openid', $oauthUser->getId())->first();
                }

                # 没有用户,默认创建
                if (!$user) {
                    $user = User::create([
                        'name' => $oauthUser->getNickname(),
                        'avatar' => $oauthUser->getAvatar(),
                        'weixin_openid' => $oauthUser->getId(),
                        'weixin_unionid' => $unionid,
                    ]);
                }

                break;
        }

        # 暂用
        return $this->response->array(['token' => $user->id]);
    }
}

```

**使用postman调试接口**

```
http://lartisan.bbs/api/socials/:social_type/authorizations
```

这里的url中:social\_type是一个postman的变量 , 可以自定义设置 , 这里为`weixin` .


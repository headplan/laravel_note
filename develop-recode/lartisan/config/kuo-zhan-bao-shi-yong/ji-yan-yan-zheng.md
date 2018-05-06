# 极验验证

[http://www.geetest.com/](http://www.geetest.com/)

官方提供的SDK

[https://github.com/GeeTeam/gt3-php-sdk](https://github.com/GeeTeam/gt3-php-sdk)

下载PHP的sdk

```
git clone https://github.com/GeeTeam/gt3-php-sdk.git
```

把class.geetestlib.php添加到app/Handlers/GeetestHandler.php中 , 添加命名空间 :

```
namespace App\Handlers;
修改类名
GeetestHandler
# 方法pre_process中的array_merge会报错,简单修改
if (($param != null) and (is_string($param))) {
    $data['user_id'] = $param;
}
```

添加极验验证配置文件

config/geetest.php

```php
<?php

return [
    'geetest_id'  =>  env('GEETEST_ID', 'id'),
    'geetest_key' =>  env('GEETEST_KEY', 'key'),
];
```

添加到.env文件中 , 并且在.env.example文件中备份 .

创建控制器 :

```
php artisan make:controller GeetestController
```

添加方法和路由

```php
public function getVerify()
{
    $handler = new GeetestHandler(config('geetest.geetest_id'), config('geetest.geetest_key'));
    $user_id = 'web';
    $status = $handler->pre_process($user_id);
    $data = array(
        'gtserver'=>$status,
        'user_id'=>$user_id
    );
    session(['geetest'=>$data]);
    return $handler->get_response_str();
}

Route::get('/getVerify', 'GeetestController@getVerify')->name('getVerify');
```




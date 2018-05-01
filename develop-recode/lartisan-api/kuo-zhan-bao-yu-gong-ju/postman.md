# PostMan

PostMan 是一款跨平台 , 方便我们调试 API 的工具 . 官网 :

```
https://www.getpostman.com/
```

#### 主要功能

打开 PostMan 界面 , 大体可以分为四个区域 , 左侧`接口集合`类似文件夹的功能 , 可以把接口保存在这里 . 右侧上中下分别是`请求地址`,`请求参数`和`请求结果`.

测试接口 :

```
http://www.weather.com.cn/data/sk/101010100.html
```

然后在左侧的接口集合 , 创建Lartisan接口文件夹 , 并把上面的测试天气接口存入 .

可以将已经调试好的接口保存下来 , 方便下次调试 , PostMan 也提供了导出导入接口的功能 , 方便分享接口给他人 , 还提供收费版 , 基本上免费版已经满足了大部分需求 .

#### 简单编写调试接口

laravel在routes/api.php中编写接口 , 这里使用了Dingo扩展 :

```php
$api = app('Dingo\Api\Routing\Router');

$api->version('v1', function ($api) {
    $api->get('version', function () {
        return response('这是v1的接口');
    });
});

$api->version('v2', function ($api) {
    $api->get('version', function () {
        return response('这是v2的接口');
    });
});
```

路由需要使用`Dingo\Api\Routing\Router`注册 . DingoApi 提供了version方法 , 用来进行版本控制 , 第一个参数是版本名称 , version 中的就是不用版本的路由 . 这里在 v1 和 v2 两个版本中 , 都注册了verison路由 , 但是响应不同 , 现在通过 PostMan 来试试 .

访问 , [http://larabbs.test/api](http://larabbs.test/api) , 这里的/api , 就是前面配置的`API_PREFIX=api`前缀 .

默认返回`这是v1的接口`, 因为前面已经配置了`API_VERSION=v1`版本 .

可以看到下面还返回了很多`laravel-debugbar`扩展的信息 , 这里关闭即可 .

配置文件config/debugbar.php

修改 :

```
'enabled' => env('APP_DEBUG', false), # 修改为
'enabled' => env('DEBUGER_ENABLE', false),
在.env文件中添加DEBUGER_ENABLE选项,用其控制debugbar工具的开关
DEBUGER_ENABLE=true
```

现在已经正常返回了信息 , 可以尝试添加header头 , 访问版本v2的信息

```
Accept: application/prs.lartisan.v2+json
```

这就是同一个URL API , 访问不同的版本的内容 . 


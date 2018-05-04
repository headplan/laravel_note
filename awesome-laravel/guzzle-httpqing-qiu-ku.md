# **Guzzle HTTP请求库**

Guzzle库是一套强大的 PHP HTTP 请求套件 .

```
composer require "guzzlehttp/guzzle:~6.3"
```

**基本用法**

```php
<?php

$client = new GuzzleHttp\Client();
$res = $client->request('GET', 'https://api.github.com/user', [
    'auth' => ['user', 'pass']
]);

// "200"
echo $res->getStatusCode();

// 'application/json; charset=utf8'
echo $res->getHeader('content-type');

// {"type":"User"...'
echo $res->getBody();


// 发送异步请求
$request = new \GuzzleHttp\Psr7\Request('GET', 'http://httpbin.org');
$promise = $client->sendAsync($request)->then(function ($response) {
    echo 'I completed! ' . $response->getBody();
});
$promise->wait();
```

**百度翻译接口请求**

```php
// 实例化 HTTP 客户端
$http = new Client;

// 初始化配置信息
$api = 'http://api.fanyi.baidu.com/api/trans/vip/translate?';
// 应用id
$appid = config('services.baidu_translate.appid');
// 应用key
$key = config('services.baidu_translate.key');
// 加些盐时间戳
$salt = time();

// 如果没有配置百度翻译，自动使用兼容的拼音方案
if (empty($appid) || empty($key)) {
    return $this->pinyin($text);
}

// 根据文档，生成 sign 签名
// http://api.fanyi.baidu.com/api/trans/product/apidoc
// appid+q+salt+密钥 的MD5值 这是签名格式
$sign = md5($appid. $text . $salt . $key);

// 构建请求参数,就是$api后面接的参数
$query = http_build_query([
    "q"     =>  $text,
    "from"  => "zh",
    "to"    => "en",
    "appid" => $appid,
    "salt"  => $salt,
    "sign"  => $sign,
]);

// 发送 HTTP Get 请求
$response = $http->get($api.$query);
// 把请求的body体json转成解析成数组
$result = json_decode($response->getBody(), true);

// 尝试获取获取翻译结果,根绝上面的数组
if (isset($result['trans_result'][0]['dst'])) {
    return str_slug($result['trans_result'][0]['dst']);
} else {
    // 如果百度翻译没有结果，使用拼音作为后备计划。
    return $this->pinyin($text);
}
```




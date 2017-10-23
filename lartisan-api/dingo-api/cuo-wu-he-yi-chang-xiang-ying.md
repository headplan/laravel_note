# 错误和异常响应

在构建API的时候处理错误是一件痛苦的事儿 , 在Dingo API中 , 你不需要手动构建错误响应 , 只需要抛出一个继承自`Symfony\Component\HttpKernel\Exception\HttpException`的异常 , API会自动为你处理这个响应 .

下面是Dingo API内置的Symfony异常 :

| Exception | Status Code |
| --- | ---: |
| `Symfony\Component\HttpKernel\Exception\AccessDeniedHttpException` | 403 |
| `Symfony\Component\HttpKernel\Exception\BadRequestHttpException` | 400 |
| `Symfony\Component\HttpKernel\Exception\ConflictHttpException` | 409 |
| `Symfony\Component\HttpKernel\Exception\GoneHttpException` | 410 |
| `Symfony\Component\HttpKernel\Exception\HttpException` | 500 |
| `Symfony\Component\HttpKernel\Exception\LengthRequiredHttpException` | 411 |
| `Symfony\Component\HttpKernel\Exception\MethodNotAllowedHttpException` | 405 |
| `Symfony\Component\HttpKernel\Exception\NotAcceptableHttpException` | 406 |
| `Symfony\Component\HttpKernel\Exception\NotFoundHttpException` | 404 |
| `Symfony\Component\HttpKernel\Exception\PreconditionFailedHttpException` | 412 |
| `Symfony\Component\HttpKernel\Exception\PreconditionRequiredHttpException` | 428 |
| `Symfony\Component\HttpKernel\Exception\ServiceUnavailableHttpException` | 503 |
| `Symfony\Component\HttpKernel\Exception\TooManyRequestsHttpException` | 429 |
| `Symfony\Component\HttpKernel\Exception\UnauthorizedHttpException` | 401 |
| `Symfony\Component\HttpKernel\Exception\UnsupportedMediaTypeHttpException` | 415 |

下面是一个示例 , 当我们尝试更新一条已经被别人更新过的记录时抛出一个`ConflictHttpException`异常 :

```
# 添加路由
$api->put('article/{id}', 'ArticlesController@update');

public function update($id)
{
    $article = Article::find($id);
    if ($article->updated_at > app('request')->get('last_updated')) {
        throw new ConflictHttpException('已经有人更新了.(这里仅仅为了模拟)');
    }
}
```

这个包自动的接住异常 , 然后转换为JSON . 响应的 HTTP 状态码也会根据异常而改变 . 如果你没有改变默认的错误格式 , 一个ConflictHttpException异常返回的结果为HTTP 409 状态码和响应的JSON表述信息 .

#### **资源异常**

以下是资源异常 , 每个异常都会返回`422`状态码 :

```
Dingo\Api\Exception\DeleteResourceFailedException
Dingo\Api\Exception\ResourceException
Dingo\Api\Exception\StoreResourceFailedException
Dingo\Api\Exception\UpdateResourceFailedException
```

这些异常特殊之处在于你可以将创建、更新或者删除资源时遇到的验证错误传递到这些异常中 .

下面我们就来看一个创建数据失败抛出`StoreResourceFailedException`异常的例子 :

```php
# 创建路由
$api->post('article', 'ArticlesController@create');

public function create(Request $request)
{
    $rules = [
        'title' => ['required', 'alpha'],
        'content' => ['required', 'min:7']
    ];

    $payload = $request->only('title', 'content');
    $validator = Validator::make($payload, $rules);

    if ($validator->fails()) {
        throw new StoreResourceFailedException('无法创建新数据', $validator->errors());
    }
}
```

Dingo API会自动捕获抛出的异常并将其转化为JSON格式 , 响应的HTTP状态码也会更改为与异常相匹配的值 , 资源异常会返回`422`状态码以及如下JSON格式错误信息 : 

```
{
  "message": "无法创建新数据",
  "errors": {
    "content": [
      "The content must be at least 7 characters."
    ]
  },
  "status_code": 422
}
```




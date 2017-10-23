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


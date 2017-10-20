# Response响应

一个API的功能主要是获取请求并返回响应给客户端 , 响应的格式是多样的 , 比如json . 返回响应的方式也是多样的 , 这取决于当前构建的API的复杂度以及对未来的考量 .

返回响应最简单的方式是直接从控制器返回数组或对象 , 但不是每个响应对象都能保证格式正确 , 所以你要确保它们实现了`ArrayObject`或者`Illuminate\Support\Contracts\ArrayableInterface`接口 , 例如我们常见的Eloquent返回的数据 :

```php
class UserController
{
    public function index()
    {
        return User::all();
    }

    public function show($id)
    {
        return User::findOrFail($id);
    }
}
```

Dingo API会自动将响应格式化为JSON格式并设置`Content-Type`头为`application/json`

#### **响应构建器**

响应构建器提供了平滑的接口以便我们轻松构建更多自定义的响应 . 响应构建器通常与转换器\(Transformer\)一起使用 . 这与我们前面自定义基础规则中创建的几个类的意思相同 . 

要使用响应构建器控制器需要使用`Dingo\Api\Routing\Helpers`trait , 为了让每个控制器都可以使用这个trait , 我们将其放置在API基类控制器`Controller`中 : 

```
use Dingo\Api\Routing\Helpers;
use Illuminate\Routing\Controller;

class ApiController extends Controller
{
    use Helpers;
}
```




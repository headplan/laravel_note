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
# 创建控制器
php artisan make:controller ApiController
```

```php
use Dingo\Api\Routing\Helpers;
use Illuminate\Routing\Controller;

class ApiController extends Controller
{
    use Helpers;
}
```

现在可以定义一个继承自该控制器的控制器 , 在这些控制器中可以通过`$response`属性来访问响应构建器 .

```
# 创建Articles控制器,这里我们将其创建到Api目录下
php artisan make:controller Api/ArticlesController
# 前面我们已经创建了,还有Article.php模型
```

下面还要用到我们前面自定义时需要用到的转换器transformer , 这里使用的是Dingo API的转换器 , 我们先简单的创建一下 : 

```php
# 创建Transformers\ArticlesTransformer.php文件
<?php

namespace App\Transformers;

use League\Fractal\TransformerAbstract;

class ArticlesTransformer extends TransformerAbstract
{
    public function transform($article)
    {
        return [
            'title' => $article['title'],
            'content' => $article['content'],
            'status' => (boolean) $article['status']
        ];
    }
}
```

编辑控制器 , 并测试Dingo API给出的响应 :

```

```




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

添加路由

```php
$api->version('v1', function ($api) {
    $api->group(['namespace'=>'App\Http\Controllers\Api'], function ($api) {
        $api->get('article/{id}','ArticlesController@show');
        $api->get('article', 'ArticlesController@index');
        $api->get('nocon', 'ArticlesController@noCon');
        $api->get('myres', 'ArticlesController@myRes');
        $api->get('myerr', 'ArticlesController@myErr');
        $api->get('myitem/{id}', 'ArticlesController@myItem');
    });
});
```

编辑控制器 , 并测试Dingo API给出的响应 :

```php
<?php

namespace App\Http\Controllers\Api;

use App\Article;
use App\Http\Controllers\ApiController;
use App\Transformers\ArticlesTransformer;

class ArticlesController extends ApiController
{
    public function show($id)
    {
        $article = Article::findOrFail($id);
        # 数组响应
//        return $this->response->array($article->toArray());
        # 单个Item响应
        return $this->response->item($article, new ArticlesTransformer());
    }

    public function index()
    {
        # 集合响应
//        $users = Article::all();
//        return $this->response->collection($users, new ArticlesTransformer);
        # 分页响应
        $users = Article::paginate(25);
        return $this->response->paginator($users, new ArticlesTransformer());
    }

    public function noCon()
    {
        # 无内容响应
        return $this->response->noContent();
    }

    public function myRes()
    {
        # 可以接收两个参数
        # location和content
        $location = '123';
        $content = '666';
        # 创建响应
        return $this->response->created($location, $content);
    }

    /**
     * 错误响应
     */
    public function myErr()
    {
        # 错误响应
//        return $this->response->error('This is an error.', 404);
        # 404错误
//        return $this->response->errorNotFound();
        # 400错误
//        return $this->response->errorBadRequest();
        # 403错误
//        return $this->response->errorForbidden();
        # 500错误
//        return $this->response->errorInternal();
        # 401错误
        return $this->response->errorUnauthorized();
    }

    /**
     * 添加额外的内容
     */
    public function myItem($id)
    {
        $article = Article::findOrFail($id);
        # 添加额外的响应头
        return $this->response->item($article, new ArticlesTransformer())
            ->withHeader('Hello', 'World');
        # 添加元数据,这里还有两个类似功能的方法
        # meta和setMeta,前者调用了addMeta,后者支持数组
//        return $this->response->item($article, new ArticlesTransformer())
//            ->addMeta('Hi','Baby');
        # 设置响应状态码
//        return $this->response->item($article, new ArticlesTransformer())
//            ->statusCode(202);
    }
}
```

#### 自定义响应格式

在安装配置中我们已经简单接触过响应格式 , 默认情况下Dingo API会自动使用JSON格式并设置相应的Content-Type头 . 除了JSON之外还有一个JSONP格式 ,该格式会将响应封装到一个回调中 . 要注册该格式只需要简单将配置文件\(Laravel\)或启动文件\(Lumen\)中的默认JSON格式替换成JSONP即可 :

```
'formats' => [
    'json' => 'Dingo\Api\Http\Response\Format\Jsonp'
]
# 或者
Dingo\Api\Http\Response::addFormatter('json', new Dingo\Api\Http\Response\Format\Jsonp);
```

> JSONP部分内容可以查看json\_note中相关记录

默认情况下 , 预计的 query string 中的回调参数是callback , 可以传递第一个参数到 class 的构造函数中去替换 . 如果 query string 中没有提供回调参数的名字 , 它将默认的返回 JSON 响应 .

如果你需要 , 你可以注册并使用你自己的 formatters . 你的 formatter 需要继承Dingo\Api\Http\Response\Format\Format . 下面的方法需要被定义 : formatEloquentModel,formatEloquentCollection,formatArray和getContentType . 你可以在预定义格式化类中得到更多的资料 , 包括提到的抽象类中每个方法需要做什么 .

#### **Morphing和Morphed事件**

在Dingo API发送响应之前会先对该响应进行转化\(morph\) , 这个过程包括运行所有转换器\(Transformer\)以及通过配置的响应格式发送响应 . 如果你需要控制响应如何被转化可以使用`ResponseWasMorphed`和`ResponseIsMorphing`事件 .

在`app/Listeners`中为事件创建监听器 , 然后在EventServiceProvider中注册即可 , 和使用Laravel的事件一样 :

```php
protected $listen = [
    'Dingo\Api\Event\ResponseWasMorphed' => [
        'App\Listeners\AddPaginationLinksToResponse'
    ]
];

<?php

namespace App\Listeners;

use Dingo\Api\Event\ResponseWasMorphed;

class AddPaginationLinksToResponse
{
    public function handle(ResponseWasMorphed $event)
    {
        if (isset($event->content['meta']['pagination'])) {
            $links = $event->content['meta']['pagination'];

            $event->response->headers->set(
                'link',
                sprintf('<%s>; rel="next", <%s>; rel="prev"', $links['links']['next'], $links['per_page'])
            );
        }
    }
}
```




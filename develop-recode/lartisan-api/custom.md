# 自定义基本规则

**新建项目**

```
composer create-project laravel/laravel ./Lartisan-API "5.4.*" --prefer-dist
```

简单配置一下.env文件

**创建数据表**

```
php artisan make:migration create_article_table --create=articles
php artisan make:model Article
php artisan make:controller ArticlesController --resource
```

创建数据库

**配置migration文件**

简单设置几个字段

```php
public function up()
{
    Schema::create('articles', function (Blueprint $table) {
        $table->increments('id');
        $table->string('title');
        $table->string('subtitle');
        $table->text('content');
        $table->boolean('status');
        $table->timestamps();
    });
}
```

生成数据表 : `php artisan migrate`

**生成测试数据**

编辑ModelFactory.php文件

```php
$factory->define(App\Article::class, function (Faker\Generator $faker) {
    return [
        'title' => $faker->title,
        'subtitle' => $faker->sentence,
        'content' => $faker->paragraph,
        'status' => $faker->boolean,
    ];
});
```

进入tinker交互界面 , 生成测试数据

```
php artisan tinker
>>> namespace App;
>>> factory(Article::class, 100)->create();
```

**配置路由**

```
Route::group(['prefix'=>'/v1'], function () {
    Route::resource('articles', 'ArticlesController');
});
```

查看路由 : `php artisan route:list`

---

**操作数据**

```php
# 控制器简单查数据
# 添加状态吗
public function index()
{
    $articles = Article::all();
    return response()->json([
        'status'      => 'success',
        'status_code' => '200',
        'data'        => $this->transformAll($articles)
    ]);
}

public function show($id)
{
    $article = Article::findOrFail($id);
    return response()->json([
        'status'      => 'success',
        'status_code' => '200',
        'data'        => $this->transform($article)
    ]);
}
# 隐藏表结构,简单添加两个转换器
private function transform($article)
{
    return [
        'title'   => $article['title'],
        'content' => $article['content'],
        'status'  => (boolean) $article['status']
    ];
}

private function transformAll($articles)
{
    return array_map([$this, 'transform'], $articles->toArray());
}

# 模型简单隐藏数据
protected $hidden = ['updated_at'];
```

**重构转换器**

```php
# 新建转换器的抽象类
# Transformers
<?php

namespace App\Transformers;

abstract class Transformers
{
    public function transformAll($items)
    {
        return array_map([$this, 'transform'], $items);
    }
    public abstract function transform($item);
}

# 之后针对不同需求,或者说不同的表进行不同的转换就可以新建一个Transformer
<?php

namespace App\Transformers;

class ArticlesTransformer extends Transformers
{
    /**
     * @param $article
     * @return array
     */
    public function transform($article)
    {
        return [
            'title'   => $article['title'],
            'content' => $article['content'],
            'status'  => (boolean) $article['status']
        ];
    }
}

# 然后依赖注入到控制器中,修改一下调用方式即可
protected $articlesTransformer;

/**
 * ArticlesController constructor.
 * @param $articlesTransformer
 */
public function __construct(ArticlesTransformer $articlesTransformer)
{
    $this->articlesTransformer = $articlesTransformer;
}
```

---

**错误与消息提示**

```php
# 请求找不到的数据时,需要错误的消息提示,简单判断即可
if ( ! $article) {
    return response()->json([
        'status'      => 'failed',
        'status_code' => '404',
        'message'     => 'Atricle Not Found'
    ]);
}
```

重构API的错误提示 , 让其更容易管理

```php
# 新建一个控制器
php artisan make:controller ApiController
# 在ApiController中统一管理响应内容,状态,错误等
<?php

namespace App\Http\Controllers;

class ApiController extends Controller
{
    protected $statusCode = 200;

    /**
     * @return int
     */
    public function getStatusCode(): int
    {
        return $this->statusCode;
    }

    /**
     * @param int $statusCode
     * @return $this
     */
    public function setStatusCode(int $statusCode)
    {
        $this->statusCode = $statusCode;
        return $this;
    }

    /**
     * @param string $message
     * @return \Illuminate\Http\JsonResponse
     */
    public function responseNotFound($message = 'Not Found')
    {
        return $this->setStatusCode(404)->responseError($message);
    }

    private function responseError($message)
    {
        return $this->response([
            'status'      => 'failed',
            'error'       => [
                'status_code' => $this->getStatusCode(),
                'message'     => $message
            ]
        ]);
    }

    protected function response($data)
    {
        return response()->json($data, $this->getStatusCode());
    }
}
```

重新调用一下ApiController中的方法,在我们的ArticlesController中 .


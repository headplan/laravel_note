# 预加载

前面提到了懒加载 , 就是第一次访问属性时 , 才会加载关联数据 . 这里的预加载 , 可以有效的避免N+1查询问题 , 在查询父模型时 , Eloquent可以预加载关联数据 .

举个例子 :

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    # 关联book的作者
    public function author()
    {
        return $this->belongsTo('App\Author');
    }
}
```

现在 , 获取所有书籍和作者数据 :

```php
$books = App\Book::all();

foreach ($books as $book) {
    echo $book->author->name;
}
```

这个循环会运行一次查询取回所有数据表上的书籍数据 , 然后又运行一次查询获得每本书的作者数据 . 如果我们有 25 本书 , 则循环就会执行 26 次查询 : 1 次是获得所有书籍数据 , 另外 25 条查询用来获得每本书的作者数据 .

这里的whith方法 , 就是为了避免这种N+1的问题 . 让查询减少到了两次 :

```php
$books = App\Book::with('author')->get();

foreach ($books as $book) {
    echo $book->author->name;
}
```

其执行的SQL为 :

```SQL
select * from books
select * from authors where id in (1, 2, 3, 4, 5, ...)
```

---

#### 预加载多个关联

在with中使用数组参数即可 : 

```php
$books = App\Book::with(['author', 'publisher'])->get();
```

#### 嵌套预加载

和前面的查询关联一样 , 也可以使用点连接的方式 , 例如 , 预加载所有书籍作者和这些作者的联系信息 : 

```php
$books = App\Book::with('author.contacts')->get();
```

#### 为预加载添加约束条件

预加载的条件约束依然可以使用闭包的方式添加 , 依然可以使用查询语句构造器的所有方法 : 

```php
$users = App\User::with(['posts' => function ($query) {
    $query->where('title', 'like', '%first%');
}])->get();
```

#### 延迟预加载

即动态的决定是否加载关联模型 , 例如 , 获得父级模型后才去进行预加载 : 

```php
$books = App\Book::all();

if ($someCondition) {
    $books->load('author', 'publisher');
}
```

延迟预加载也支持条件约束 , 还是使用闭包的方式 : 

```php
$books->load(['author' => function ($query) {
    $query->orderBy('published_date', 'asc');
}]);
```




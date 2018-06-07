# 集合

`Illuminate\Support\Collection`类提供了一个更具可读性的、更便于处理数组数据的封装 .

举个简单的例子 :

```php
$collection = collect(['taylor', 'abigail', null])->map(function ($name) {
    return strtoupper($name);
})
->reject(function ($name) {
    return empty($name);
});
```

一般来说 , 为了保证集合数据的纯洁性 , 都会返回一个全新的`Collection`实例 .

#### 创建集合

使用辅助函数`collect`会为给定的数组返回一个新的`Illuminate\Support\Collection`实例 .

```php
$collection = collect([1, 2, 3]);
```

通常 , Eloquent查询的结果返回的内容都是`Collection`实例 . 就是`Illuminate\Database\Eloquent\Collection`实例 . 这个类继承了集合基类 , 也就是`Illuminate\Support\Collection`类 :

```php
use Illuminate\Support\Collection as BaseCollection;
```

所以 , 基类中的操作方法也都继承了 .

**需要注意的是 : **

> 大多数 Eloquent 集合方法会返回新的 Eloquent 集合实例 . 但是 `pluck`, `keys`, `zip`, `collapse`, `flatten` 和 `flip` 方法除外 , 它们会返回 **集合基类** 实例 . 其实 , 如果`map`操作返回的集合不包含任何 Eloquent 模型 , 那么它也会被自动转换成**集合基类** .

#### 扩展集合

通过macro宏 , 添加一些自定义的集合方法 :

```php
use Illuminate\Support\Str;

Collection::macro('toUpper', function () {
    return $this->map(function ($value) {
        return Str::upper($value);
    });
});

$collection = collect(['first', 'second']);

$upper = $collection->toUpper();

// ['FIRST', 'SECOND']
```

在Eloquent中想在自己的扩展方法中使用自定义的`Collection`对象 , 可以在自己的模型中重写`newCollection`方法 , 或者在统一继承的Model中写 . 

```php
<?php

namespace App;

use App\CustomCollection;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * 创建一个新的 Eloquent 集合实例对象。
     *
     * @param  array  $models
     * @return \Illuminate\Database\Eloquent\Collection
     */
    public function newCollection(array $models = [])
    {
        return new CustomCollection($models);
    }
}
```




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




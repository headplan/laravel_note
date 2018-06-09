# 分页

分页其实很简单 , 将分页实例传递给资源的`collection`方法或者自定义的资源集合即可 , 前面也提到了 , 分页的meta和link数组 , 总是包含在响应数据中的 . 

```php
use App\User;
use App\Http\Resources\UserCollection;

Route::get('/users', function () {
    return new UserCollection(User::paginate());
});
```




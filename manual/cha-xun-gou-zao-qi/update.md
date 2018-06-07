# Updates

更数据 , 使用update方法 , 其用法和insert一样 , 一般使用where方法来约束 : 

```php
DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);
```

#### 更新JSON字段

这里只要记住在支持JSON的数据上 , 并且使用`->`访问json对象中的相应键即可 . 

#### 自增 & 自减

increment和decrement方法 . 两个方法都接受三个参数 : 

只写第一个参数 , 表示加1或减1 . 

第二个参数表示递增或递减的量 . 

第三个参数可以指定要更新的字段 . 

```php
DB::table('users')->increment('votes', 5);

DB::table('users')->decrement('votes');

DB::table('users')->increment('votes', 1, ['name' => 'John']);
```




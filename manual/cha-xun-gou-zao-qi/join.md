# Join

#### Inner Join 语句

执行基本的「内连接」, 可以在查询构造器实例上使用`join`方法 . 传递给`join`方法的第一个参数是需要连接的表的名称 , 而其它参数则用来指定连接的字段约束 . 还可以在单个查询中连接多个数据表 : 

```php
$users = DB::table('users')
            ->join('contacts', 'users.id', '=', 'contacts.user_id')
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.*', 'contacts.phone', 'orders.price')
            ->get();
```




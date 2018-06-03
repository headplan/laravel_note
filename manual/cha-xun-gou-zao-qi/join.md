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

#### Left Join 语句

如果想用「左连接」来代替「内连接」, 使用`leftJoin`方法 . `leftJoin`方法的用法和`join`方法一样 : 

```php
$users = DB::table('users')
            ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();
```

#### Cross Join 语句

使用`crossJoin`方法和想要交叉连接的表名来做「交叉连接」. 交叉连接在第一个表和连接之间生成笛卡尔积 : 

```php
$users = DB::table('sizes')
            ->crossJoin('colours')
            ->get();
```




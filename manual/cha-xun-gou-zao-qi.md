# 查询构造器

Laravel 的数据库查询构造器提供了一个方便的接口来创建及运行数据库查询语句 . Laravel 的查询构造器使用 PDO 参数绑定来保护你的应用程序免受 SQL 注入的攻击 , 所以没有必要清理传递的字符串参数 .

### 获取结果

#### 从数据表中获取所有数据

```php
$users = DB::table('users')->get();
```

其中get\(\)返回的是Collection , 其中的结果都是个StdClass , 可以使用对象的方式访问数据 : 

```php
foreach ($users as $user) {
    echo $user->name;
}
```




# Union

查询合并数据 , 是一种快捷方式 , 先创建一个初始查询 , 再使用union方法将它与第二个查询进行合并 :

```php
$first = DB::table('users')
            ->whereNull('first_name');

$users = DB::table('users')
            ->whereNull('last_name')
            ->union($first)
            ->get();
```

也可以使用unionAll\(\)方法 , 和Union一样 . 


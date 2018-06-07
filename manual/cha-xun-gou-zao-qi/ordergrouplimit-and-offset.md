# Order,Group,limit & Offset

#### orderBy

根据指定字段对查询结果进行排序 .

第一个参数是想要用来排序的字段 .

第二个参数是控制排序的方向 .

```php
$users = DB::table('users')
                ->orderBy('name', 'desc')
                ->get();
```

#### latest/oldest

这两个方法可以轻松的按照日期对查询结果排序 . 默认情况下对created\_at字段进行排序 .

当然也可以在参数中指定字段 :

```php
$user = DB::table('users')->latest()->first;
```

#### inRandomOrder

这个方法可以将查询结果随机排序 . 例如 , 可以使用这个方法获取一个随机用户 :

```php
$randomUser = DB::table('users')->inRandomOrder()->first();
```

#### groupBy/having

这两个方法用来对查询结果进行分组 . having的用法和where方法类似 :

```php
$users = DB::table('users')
                ->groupBy('account_id')
                ->having('account_id', '>', 100)
                ->get();
```

groupBy方法可以接收多个参数 , 按照多个字段进行分组 :

```php
$users = DB::table('users')
                ->groupBy('first_name', 'status')
                ->having('account_id', '>', 100)
                ->get();
```

要写一些更高级的语句 , 可以使用havingRaw . 

#### skip/take

使用skip和take方法 , 跟使用offset和limit方法一样 . 都可以限制查询返回的结果数量和跳过查询中给定的数量的结果 : 

```php
$users = DB::table('user')->skip(10)->take(5)->get();

$users = DB::table('user')->offset(10)->limit(5)->get();
```




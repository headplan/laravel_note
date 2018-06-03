# 获取结果

```
get()
first()
value()
pluck()
chunk()
count、max、min、avg、sum
```

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

#### 从数据表中获取单个列或行

获取一行数据 , 使用first\(\)函数 :

```php
$user = DB::table('users')->where('name', 'John')->first();
```

返回一个`StdClass`对象 .

直接获取一个字段的值

```php
$email = DB::table('users')->where('name', 'John')->value('email');
```

获取一列的值 , 使用pluck :

```php
$titles = DB::table('roles')->pluck('title');

foreach ($titles as $title) {
    echo $title;
}
```

这里的pluck接受多个参数 :

```php
$roles = DB::table('roles')->pluck('title', 'name');

foreach ($roles as $name => $title) {
    echo $title;
}
```

分块处理 , 也就是一次从表中取出一块 , 来处理 , 直到完成 , 可以返回false阻止 :

```php
DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    foreach ($users as $user) {
        //
    }
});
```

常见的聚合函数

`count`、`max`、`min`、`avg`和`sum`

```php
$price = DB::table('orders')->max('price');
```




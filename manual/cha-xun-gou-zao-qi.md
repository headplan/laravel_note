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




# Inserts

insert方法参数同样接受一条和多条的插入 :

```php
DB::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);
```

插入多条记录 :

```php
DB::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
]);
```

自增ID : 

使用insertGetId方法 , 插入记录后获取其ID : 

```php
$id = DB::table('users')->insertGetId(
    ['email' => 'john@example.com', 'votes' => 0]
);
```

> PostgreSQL默认把id当成自增id , 可以用第二个参数指定字段名 .




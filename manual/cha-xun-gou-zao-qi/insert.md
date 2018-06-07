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




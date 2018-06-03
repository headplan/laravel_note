# Select

```
select()
distinct()
addSelect()
```

**指定字段查询**

```php
$users = DB::table('users')->select('name', 'email as user_email')->get();
```

**去重查询**

```php
$users = DB::table('users')->distinct()->get();
```

**在查询构造器实例上增加一个select的字段**

```php
$query = DB::table('users')->select('name');
$users = $query->addSelect('age')->get();
```




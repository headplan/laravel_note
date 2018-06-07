# Delete

和前面的insert和update方法一样 , delete方法可以删除记录 . 一般删除前 , 应该添加where条件约束一下 : 

```php
DB::table('users')->delete();

DB::table('users')->where('votes', '>', 100)->delete();
```

清空数据表  , 使用truncate\(\)方法 : 

```php
DB::table('users')->truncate();
```




# 悲观锁

使用`sharedLock`方法 . 共享锁可以防止选中的行被篡改 , 直到事务被提交为止 : 

```php
DB::table('users')->where('votes', '>', 100)->sharedLock()->get();
```

还可以使用lockforUpdate方法 , 使用更新锁避免行被其他共享锁修改或选取 : 

```php
DB::table('users')->where('votes', '>', 100)->lockForUpdate()->get();
```




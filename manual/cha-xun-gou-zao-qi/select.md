# Select

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

**原生表达式**

```php
->select(DB::raw('count(*) as user_count, status'))
```

DB::raw\(\)的参数是字符串 , 所以要注意注入的可能 . 

**原生方法**

#### `selectRaw`

使用`selectRaw`代替`DB::raw()` , 第二个参数可以使用参数绑定 , 是一个数组

```php
->selectRaw('price * ? as price_with_tax', [1.0825])
```

#### `whereRaw / orWhereRaw`

这两个方法可以写原生where , 第二个参数也是绑定的参数 . 

```php
->whereRaw('price > IF(state = "TX", ?, 100)', [200])
```

#### `havingRaw / orHavingRaw`

```php
->havingRaw('SUM(price) > 2500')
```

#### `orderByRaw`

```php
->orderByRaw('updated_at - created_at DESC')
```




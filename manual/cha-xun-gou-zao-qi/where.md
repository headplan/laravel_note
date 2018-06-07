# Where

#### 简单的where语句

基本的where方法需要三个参数 , 第四个参数默认为and .

```php
public function where($column, $operator = null, $value = null, $boolean = 'and')
```

第一个参数 , 字段名 ;

第二个参数 , 运算符 ;

第三个参数 , 值 ;

第四个参数 , 默认and ;

其中第二个参数直接写值的话 , 就是等于的意思 .

```php
$users = DB::table('users')->where('votes', 100)->get();

$users = DB::table('users')
                ->where('votes', '<>', 100)
                ->get();

$users = DB::table('users')
                ->where('name', 'like', 'T%')
                ->get();
```

参数也可以是数组

```php
$users = DB::table('users')->where([
    ['status', '=', '1'],
    ['subscribed', '<>', '1'],
])->get();
```

#### Or语句

这里的orWhere方法和where方法一样 , 实现也是调用where方法 , 只是把where默认的第四个参数改为了or :

```php
$users = DB::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();
```

后面的和where相关的语句 , 基本都包含了Or语句 , 其实现方式基本上都是设置where方法的默认的第四个参数 .

#### 其他where语句

其他where语句基本都是字面意思 .

**whereBetween\(\) / whereNotBetween\(\) - **验证字段的值是否介于两个值之间 , 参数为字段名和一个数组 , 例如\[1,100\] .

**whereIn\(\) / whereNotIn\(\) - **验证字段是否存在于指定数组中 . 参数为字段名和一个数组 , 例如\[1,2,3\];

**whereNull\(\) / whereNotNull\(\) - **验证字段的值是否为NULL .

**whereDate / whereMonth / whereDay / whereYear / whereTime - **比较指定字段的值和日期时间等 , 参数和where的一样 .

**whereColumn\(\) - **用于验证两个字段是否相等 . 接收参数与where方法一样 , 支持两个的默认等于 , 和三个的自定义运算符 , 参数也可以是数组 .

```php
$users = DB::table('users')
                ->whereColumn([
                    ['first_name', '=', 'last_name'],
                    ['updated_at', '>', 'created_at']
                ])->get();
```

---

#### 参数分组

顾名思义 , 通过分组的方式约束条件参数 , 这里闭包传递了一个查询构造器 , 分组就相当于sql语句的括号 .

```php
DB::table(''users)->where('name', '=', 'John')
                ->orWhere(function ($query) {
                    $query->where('votes', '>', 100)
                          ->where('title', '<>', 'Admin')
                })
                ->get();
```

#### Where Exists 语句

whereExists方法 , 相当于`where exists`SQL语句 . 此方法接受一个**闭包**参数 , 此闭包要接收一个查询构造器实例 , 放在exists语句中查询 : 

```php
DB::table('users')
            ->whereExists(function ($query) {
                $query->select(DB::raw(1))
                      ->from('orders')
                      ->whereRaw('orders.user_id = users.id');
            })
            ->get();
```

#### JSON where 语句

json类型的字段 , 仅仅在对JSON类型支持的数据库上 . 现在只支持MySQL5.7+和Postgres数据库 . 可以使用`->`运算符来查询JSON列数据 : 

```php
$users = DB::table('users')
                ->where('options->language', 'en')
                ->get();

$users = DB::table('users')
                ->where('preferences->dining->meal', 'salad')
                ->get();
```




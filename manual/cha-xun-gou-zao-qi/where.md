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

#### Or语句

#### 其他where语句

---

#### 参数分组

#### Where Exists 语句

#### JSON where 语句




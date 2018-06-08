# 修改器

在 Eloquent 模型实例中获取或设置某些属性值的时候 , 访问器和修改器允许对 Eloquent 属性值进行格式化 . 例如 , 设置密码的时候 , 或者转换日期的时候 .

#### 定义访问器

这里假如要对first\_name字段定义访问器 , 在访问字段时自动首字母大写 :

```php
public function getFirstNameAttribute($value)
{
    return ucfirst($value);
}
```

现在直接`$this->first_name`时候就会调用访问器了 . 还可以使用其他属性组合成新的属性 , 比如全名 :

```php
public function getFullNameAttribute()
{
  return "{$this->first_name} {$this->last_name}";
}
```

#### 定义修改器

定义修改器和前面的一样 , 只是变成了set :

```php
public function setFirstNameAttribute($value)
{
    $this->attributes['first_name'] = strtolower($value);
}

====================

$user = App\User::find(1);

$user->first_name = 'Sally';
```

定义好修改器 , 现在设置模型的属性 , 会自动调用修改器 , 并把其设置到模型attributes属性上 . 上面的例子 , 设置的Sally值会经过小写的处理 , 在赋给attributes .

#### 日期转换器

通过重写模型的`$dates`属性 , 自行定义哪些日期类型字段会被自动转换 , 或者完全禁止所有日期类型字段的转换 . 默认是`created_at`和`updated_at`字段 .

```php
protected $dates = [
    'created_at',
    'updated_at',
    'deleted_at'
];
```

正如上面的设置 , 现在这三个时间属性 , 都自动转换成Carbon实例 . 存取的时候 , 直接用Carbon实例就可以 , 当然也可以存其他的时间戳 , 日期 , 日期字符串 , DateTime实例等等 .

```php
$user = App\User::find(1);

$user->deleted_at = Carbon::now();

$user->save();

return $user->deleted_at->getTimestamp();
```

**日期格式**

默认情况下 , 时间戳将会以`'Y-m-d H:i:s'`的形式格式化 . 自定义的话可以在模型中设置`$dateFormat`属性 , 这个属性设置的是保存在数据表和最后序列化成数组或是JSON时候的日期格式 .

```php
class User extends Model
{
    /**
     * 模型的日期字段的保存格式。
     *
     * @var string
     */
    protected $dateFormat = 'U';
}
```

#### 属性类型转换

在模型中定义`$casts`属性 , 可以方便的将字段值的数据类型转换成定义好的数据类型 . `$casts`属性是一个数组 , 且数组的键就是字段名 , 值是数据类型 . 下面是支持转换的数据类型 :

```php
integer
real
float
double
string
boolean
object
array
collection
date
datetime
timestamp
```

举个例子 , 将is\_admin字段的0,1转换成布尔型 :

```php
protected $casts = [
    'is_admin' => 'boolean',
];
```

设置完之后 , 再访问属性的时候 , 就会自动转换为bool型 : 

```php
$user = App\User::find(1);

if ($user->is_admin) {
    //
}
```




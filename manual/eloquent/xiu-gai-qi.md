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






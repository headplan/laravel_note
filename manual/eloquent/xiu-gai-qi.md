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




# 添加参数到JSON中

在转换模型到数组或JSON时 , 添加一个在数据库中没有对应字段的属性 . 

首先 , 定义访问器 , 访问器的名称是基于 「Camel Case」 的命名方式 , 而属性名称通常遵循 「Snake Case」的命名方式 : 

```php
public function getIsAdminAttribute()
{
    return $this->attributes['admin'] == 'yes';
}
```

然后将属性添加到模型的`$append`属性中即可 : 

```php
protected $appends = ['is_admin'];
```




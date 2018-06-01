# Eloquent

### 关联

#### 定义关联

**一对一**

```php
public function phone()
{
    return $this->hasOne('Phone::class');
}
```

一个用户模型对应一个手机模型 . 例如一个用户有一个手机号 . 

现在就可以动态获取了 : 

```php
$phone = User::find(1)->phone;
```




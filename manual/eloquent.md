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

hasOne支持三个参数 :

```php
return $this->hasOne('App\Phone', 'foreign_key', 'local_key');
```

第二个参数 : 外键名 , Eloquent会基于**模型名**决定外键名称 , 这里即user\_id , 也可以自定义其他的 .

第三个参数 : 关联ID名 , Eloquent默认关联id , 也就是user\_id中关联user模型中的id主键 , 定义这个可以关联其他的user模型字段.

现在就可以动态获取了 :

```php
$phone = User::find(1)->phone;
```

**一对一反向关联**

hasOne方法对应的方法belongsto方法定义反向关联 .

```php
public function user()
{
    return $this->belongsTo('User::class');
}
```

Eloquent 会尝试匹配 Phone 模型上的 user\_id 至 User 模型上的 id .

根据关联的方法名 , 这里即user . 拼接字符串`_id`来确定关联的id , 最后拼接为外键名称user\_id .

belongsTo方法支持四个参数 :

```php
public function belongsTo($related, $foreignKey = null, $ownerKey = null, $relation = null)
```

第一个参数即父类命名空间 .

第二个参数是指定外键名称 , 这里是user\_id .

第三个参数是指定要关联的父类的字段 , 可以理解为自定义键 .

第四个参数相当于使用了with参数 , 其中使用其他模型 .

**默认模型**

belongsTo允许使用默认模型 , 这种设计模式通常称为空对象 , 默认返回null , 也可以定义没有找到对应父类内容时的默认值 .

免去了判断的部分 .

使用withDefault\(\)方法 , 其接受数组和闭包的方式 .

```php
public function user()
{
    return $this->belongsTo('App\User')->withDefault();
}
```

```php
public function user()
{
    return $this->belongsTo('App\User')->withDefault(function ($user) {
        $user->name = 'test';
        $user->phone = 123;
    });
}
```

---

**一对多**

关联用于定义单个模型拥有任意数量的其它关联模型 , 例如一篇文章有无限多条评论 .

例如在文章Article模型中定义评论Comment关联 .

```php
public function comments()
{
    return $this->hasMany('App\Comment');
}
```

调用方法和参数和hasOne方法一样 , 都是默认以snake+\_id的方式关联父级模型的id主键 . 

访问也可以链式操作 .

```php
return $this->hasMany('App\Comment', 'foreign_key', 'local_key');
```




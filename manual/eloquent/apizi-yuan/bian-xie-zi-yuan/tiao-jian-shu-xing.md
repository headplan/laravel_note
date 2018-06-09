# 条件属性

在给定条件满足时 , 添加属性到资源响应里 , 可以使用when方法 , 添加条件 . 例如 , 当用户为管理员时 , 添加某个值到资源响应里 :

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        'secret' => $this->when($this->isAdmin(), 'secret-value'),
        'created_at' => $this->created_at,
        'updated_at' => $this->updated_at,
    ];
}
```

这里 , 只有当`$this->isAdmin()`为`true`时候 , `secret`键值才会在最终的资源响应里出现 . 否则 , 在发送响应前就会被删除掉 .

这里when方法的第二个参数还可以是一个闭包函数 , 即条件为true时 , 才会返回闭包中的值 .

```php
'secret' => $this->when($this->isAdmin(), function () {
    return 'secret-value';
}),
```

需要注意的是 , 这里 , 就是在资源上调用的方法 , 都是代理到底层Eloquent方法上的模型实例的方法 , 所以这里的`isAdmin()`方法 , 实际上是定义在模型上的方法 .

#### 有条件的合并数据

在满足给定条件时 , 添加多个属性到响应中 , 这里使用的方法是mergeWhen\(\)方法 , 和前面的使用方式一样 , 只是第二个参数接受的是一个数组 , 这个数组不接受数字键的数组 , 应该使用关联数组 :  

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        $this->mergeWhen($this->isAdmin(), [
            'first-secret' => 'value',
            'second-secret' => 'value',
        ]),
        'created_at' => $this->created_at,
        'updated_at' => $this->updated_at,
    ];
}
```




# 隐藏来自JSON的属性

限制包含在模型数组或 JSON 表示中的属性 , 在模型中添加`$hidden`属性即可, 与只对应的还有一个白名单属性 , `$visible`也可以 : 

```
protected $hidden = ['password'];
# 或者
protected $visible = ['first_name', 'last_name'];
```

注意 : 

> 当你要对关联进行隐藏时 , 需使用关联的**方法名称** , 而不是它的动态属性名称 .




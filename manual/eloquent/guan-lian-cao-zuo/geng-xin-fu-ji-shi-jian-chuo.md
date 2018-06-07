# 更新父级时间戳

当一个模型belongsTo\(belongsToMany\)另一个模型时 , 比如一个Comment属于一个Post , 需要在更新子模型时候更新其父模型时间戳 . 就是当 , Comment模型更新时 , 自动触发其父级Post模型的updated\_at时间戳也更新 , 只需要在子模型中添加一个包含关联名称的touches属性即可开启 : 

```php

```




# 配置文件详解

**ide-helper.php : **

```
<?php
return array(
# 默认的文件名(不带扩展名)
# 默认的格式(php或json)
'filename'  => '_ide_helper',
'format'    => 'php',
```

    # 设置为true,生成常用的Fluent方法(也就是常说的方法链,比如设计模式中的流接口模式Fluent Interface)
    'include_fluent' => true,
    # 设置为true会在_ide_helper文件中添加下面的内容
    > namespace Illuminate\Support {
    >     /**
    >      * Methods commonly used in migrations
    >      *
    >      * @method Fluent after(string $column) Add the after modifier
    >      * @method Fluent charset(string $charset) Add the character set modifier
    >      * @method Fluent collation(string $collation) Add the collation modifier
    >      * @method Fluent comment(string $comment) Add comment
    >      * @method Fluent default(mixed $value) Add the default modifier
    >      * @method Fluent first() Select first row
    >      * @method Fluent index(string $name = null) Add the in dex clause
    >      * @method Fluent on(string $table) `on` of a foreign key
    >      * @method Fluent onDelete(string $action) `on delete` of a foreign key
    >      * @method Fluent onUpdate(string $action) `on update` of a foreign key
    >      * @method Fluent primary() Add the primary key modifier
    >      * @method Fluent references(string $column) `references` of a foreign key
    >      * @method Fluent nullable() Add the nullable modifier
    >      * @method Fluent unique(string $name = null) Add unique index clause
    >      * @method Fluent unsigned() Add the unsigned modifier
    >      * @method Fluent useCurrent() Add the default timestamp value
    >      */
    >     class Fluent {}
    > }






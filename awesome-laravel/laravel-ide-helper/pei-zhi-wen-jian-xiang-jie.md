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

```
# 开启关闭模型的魔术方法
'write_model_magic_where' => true,
# 设置为true会在_ide_helper_models文件中添加下面的内容
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost whereContent($value)
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost whereCreatedAt($value)
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost whereId($value)
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost wherePublishedAt($value)
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost whereSlug($value)
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost whereTitle($value)
 * @method static \Illuminate\Database\Eloquent\Builder|\App\Models\BlogPost whereUpdatedAt($value)
```

```
'include_helpers' => false,

'helper_files' => array(
    base_path().'/vendor/laravel/framework/src/Illuminate/Support/helpers.php',
),

# 生成helper文档
# 可以在数组中添加自定义路径
# 也可以直接在命令行中添加-H(--helpers)生成
```

```
'model_locations' => array(
    'app',
),
# 生成模型文档时的遍历的路径
```

```
# 扩展类
# 这些实现不是真正的扩展,而是用魔术方法调用的
'extra' => array(
    'Eloquent' => array('Illuminate\Database\Eloquent\Builder', 'Illuminate\Database\Query\Builder'),
    'Session' => array('Illuminate\Session\Store'),
),

'magic' => array(
    'Log' => array(
        'debug'     => 'Monolog\Logger::addDebug',
        'info'      => 'Monolog\Logger::addInfo',
        'notice'    => 'Monolog\Logger::addNotice',
        'warning'   => 'Monolog\Logger::addWarning',
        'error'     => 'Monolog\Logger::addError',
        'critical'  => 'Monolog\Logger::addCritical',
        'alert'     => 'Monolog\Logger::addAlert',
        'emergency' => 'Monolog\Logger::addEmergency',
    )
),
```

```
# 接口实现
# 这些接口将被实现类替换.某些接口是由helpers检测到的,其他的则可以在下面的数组中列出
'interfaces' => array(

),
```

```
# 自定义支持类型
# 此设置允许映射任何自定义数据库类型.配置中的所有数组键都是Doctrine2 DBAL平台中提供的名称.
# 由Doctrine/DBAL/Platforms/AbstractPlatform的自方法getName()返回
# 数组的配置可以查看http://doctrine-dbal.readthedocs.org/en/latest/reference/types.html配置
# 这里举例配置
'custom_db_types' => array(
    "postgresql" => array(
        "jsonb" => "json_array",
    ),
),
```

```
'model_camel_case_properties' => false,
# 设置驼峰模式属性
# 普通配置
* @property \Carbon\Carbon $created_at
* @property \Carbon\Carbon $updated_at
# 设置后
* @property \Carbon\Carbon $createdAt
* @property \Carbon\Carbon $updatedAt
```

```
# 属性强制转换
# 将给定的真实类型转换为给定的类型
'type_overrides' => array(
   'integer' => 'int',
   'boolean' => 'bool',
),
```




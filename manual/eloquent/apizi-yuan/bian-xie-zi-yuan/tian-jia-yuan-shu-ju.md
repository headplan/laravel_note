# 添加元数据

添加元数据前面也提到过 , 直接在toArray方法中添加接口 , 而且不用担心添加一些links , meta键的数据时会和分页冲突 , 自定义添加的元数据 , 或其他数据 ,都会自动和默认的links或者meta数据合并 : 

```php
public function toArray($request)
{
    return [
        'data' => $this->collection,
        'links' => [
            'self' => 'link-value',
        ],
    ];
}
```

顶层元数据

构造资源时添加元数据


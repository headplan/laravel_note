# 用户数据

### 获取个人信息

#### Fractal

Fractal是一个转换层\(transformer\) , API 开发中非常方便的一种开发方法 , 可以帮助我们处理响应数据的结构与复杂的嵌套关系 , 最后将数据返回给客户端 . 可以把 Fractal 理解为 Web 开发中视图 , 控制着 API 的最终数据输出 .

> Laravel 5.5 的新功能`eloquent-resources`整体思路跟`Fractal`一致 , 用法也基本相同 .

#### 数据结构

Fractal提供了3中基本机构 :

* DataArraySerializer 结构类似 eloquent-resources 的 Data Wrapping
* ArraySerializer 结构类似 eloquent-resources 的 withoutWrapping
* JsonApiSerializer 结构出自JSON-API , 是一套 json 接口响应规范

常用的结构是 DataArraySerializer 和 ArraySerializer .

DataArraySerializer

```php
# 带着 meta 信息的单条数据
[
    'data' => [
        'foo' => 'bar'
    ],
    'meta' => [
        ...
    ]
];

# 带着 meta 信息的数据集合
[
    'data' => [
        [
            'foo' => 'bar'
        ]
    ],
    'meta' => [
        ...
    ]
];
```




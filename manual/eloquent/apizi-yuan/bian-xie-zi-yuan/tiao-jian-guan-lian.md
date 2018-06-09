# 条件关联

根据模型关联是否已加载来有条件的在你的资源响应中包含关联 . 避免了N+1的问题 , 还有就是可以有条件的加载关联 , 避免了没必要的加载 :

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        'posts' => Post::collection($this->whenLoaded('posts')),
        'created_at' => $this->created_at,
        'updated_at' => $this->updated_at,
    ];
}
```

这里的`Post::collection($this->whenLoaded('posts'))` , 在关联没被加载时 , posts键就会删除 .

#### 条件中间表信息

对于多对多关联 , 这种有条件的添加资源需要使用whenPivotLoaded方法 , 第一个参数是中间表的名称 , 第二个参数是闭包 , 用来返回数据 : 

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'expires_at' => $this->whenPivotLoaded('role_users', function () {
            return $this->pivot->expires_at;
        }),
    ];
}
```




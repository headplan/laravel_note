# Response响应

一个API的功能主要是获取请求并返回响应给客户端 , 响应的格式是多样的 , 比如json . 返回响应的方式也是多样的 , 这取决于当前构建的API的复杂度以及对未来的考量 . 

返回响应最简单的方式是直接从控制器返回数组或对象 , 但不是每个响应对象都能保证格式正确 , 所以你要确保它们实现了`ArrayObject`或者`Illuminate\Support\Contracts\ArrayableInterface`接口 , 例如我们常见的Eloquent返回的数据 : 

```
class UserController
{
    public function index()
    {
        return User::all();
    }
    
    public function show($id)
    {
        return User::findOrFail($id);
    }
}
```






# 利用macro方法来扩展Laravel的基础类

> Laravel利用PHP的特性 , 编写了一套叫做Macroable的Traits . 这样 , 凡是使用Macroable的类 , 都是可以使用这个方法扩充 .

**使用方法**

```
Route::macro('yo', function () {
    $this->match(['GET', 'POST'], 'yo', function () {
        return 'Yo!'.date('Y-m-d H:i:s',time());
    });
});
Route::yo();
```

这里的$this指向的是marco扩充的类 . 查看Marcoable源码

```
static::$macros[$method]->bindTo($this, static::class)
```

其中`bindTo`改变了$this上下文指向的方法 .

**服务提供者**

可以创建服务提供者 , 在boot中存放 , Marcoable的扩充类 .

**可以使用Marcoable扩展的类**

* Response
* Request
* Translator
* Collection
* Arr
* HTML
* Str
* Form
* Cache
* Filesystem




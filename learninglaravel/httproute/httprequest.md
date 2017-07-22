# HTTP\_Request

Laravel中的请求被封装成Illuminate\Http\Request类实例 , 继承Symfony对应的类 , 整个应用程序执行过程中 , 会被自动创建 . 对于请求来说 , 主要是通过请求实例获取想要的数据和参数 .

操作请求要获取请求实例 , 请求实例创建成功后会在服务器容器中进行备份 , 其过程是在入口文件的$kernel-&gt;handle\(...\);过程中完成的 , 即在服务容器中以实例对象的形式存储`$this->app->instance('request', $request);`

服务容器中有备份 , 后面就可以借助服务容器使用下面三种方法获取它 :

* Facade外观\(门面\)获取 - Reuqest::all\(\)
  * 当然这种方式无法获取类实例 , 但可以任意调用类实例的方法
  * 需要注意的问题就是自动加载的是Facade而不是命名空间下的类
* 直接从服务容器中获取 - app\("request"\);
  * 全局函数app\(\) , 定义在Illuminate\Foundation\helper.php中 , 其实它的实现就是make\('request'\)得到类实例
* 依赖注入获取
  * 依赖注入 , 可以路由回调注入 , 也可以在控制器中注入 , 其依赖的是Http下的类
  * 查看Routing下的ControllerDidpatcher.php文件 , 其中的resolveClassMethodDependencies\(\)函数 , 其在一个trait中 , 最后可以看到其也是通过$this-&gt;container-&gt;make\(...\);实现的 . 

#### 属性参数的获取和数据的存储

标题写的也就是请求实例对象的主要操作 .

**对象实例常用属性 : **

* $request - 请求参数 , 相当于$\_POST
* $query - 查询参数 , 相当于$\_GET
* $server - 服务器及环境参数 , 相当于$\_SERVER
* $files - 上传文件参数 , 相当于$\_FILES
* $cookies - 请求cookie参数 , 相当于$\_COOKIE
* $headers - 请求头部字段
* $session - session数据
* $pathInfo - 请求URL
* $method - 请求方法

**常用属性获取方法 : **查看Auth下的RegisterController类重写的register方法

```
# 获取请求路径
$uri      = $request->path();
# 验证收到的请求路径和指定规则是否匹配
$is       = $request->is('register/*');
# 获取请求的 URL
$url      = $request->url();
# 包含查询字符串
$fullUrl  = $request->fullUrl();
# 获取请求方法
$method   = $request->method();
# 验证HTTP的请求方式
$isMethod = $request->isMethod('post');
# 获取所有输入数据
$all      = $request->all();
# 获取查询字符串
$query    = $request->query();
# 获取input输入值
$input    = $request->input();
# 获取指定表单数据
$email    = $request->input('email');
# 获取指定表单数据,默认值
$emailD   = $request->input('email','默认值');
# 获取指定数组表单数据
$inputArr = $request->input('email.0',"['abc','def']");
# 通过动态属性获取输入数据
# 先从请求的数据中查找,没有的话再到路由参数中找。
$name     = $request->name;
# 获取JSON输入信息
# 请求表头中要设置Content-Type为application/json
# 点语法获取json数组
$json     = $request->input('user.name');
# 获取部分输入数据
# 不存在则返回null
$only     = $request->only('email','password','null');
# 不存在剔除
$intersect = $request->intersect('email','password','null');
$except   = $request->except(['email','password']);
$has      = $request->has('email');
```

上面这些方法都在一个trait文件InteractsWithInput.php中 , 所有请求参数都分类存储在ParameterBag类或以其为基类的实例对象中 .

**请求参数的一次存储**

请求参数有时不仅来自本次请求 , 还需要利用上一次请求的信息 . HTTP协议无状态的原因 , 保存上一次请求的输入信息 , 需要使用session或cookie的方式实现 . Laravel框架的请求类提供了对于请求参数的一次性存储接口 , 即将请求参数存储到一次性session中 .

> Larave的验证特性会自动调用这些一次性存储接口 , 不需要手动实现 .

一次性存储接口 :

* $request-&gt;flash\(\) - 方法会将当前输入的数据存进session中 , 因此下次用户发送请求到应用程序时就可以使用它们 . 
* $request-&gt;flashOnly\(\) - 将当前请求数据的子集保存到session中 , 用法和input\(\)一样 . 
* $request-&gt;flashExcept\(\) - 同上
* $request-&gt;flush\(\) - 和flash方法对应 , 清空所有input数据 , 查看trait的实现可以看出区别仅仅是`$this->input()`

这些方法都定义在一个trait文件InteractsWithFlashData中 , 它被use在Request类里 .

**闪存数组到session后重定向**

意思是重定向到页面后 , 数据也带过去 , 不然一次就没了 :

```
return redirect('form')->withInput();

return redirect('form')->withInput(
    $request->except('password')
);
```

**获取闪存数据**

* $request-&gt;old\(\) - 这里获取的数据也是从session中获取的 , 也在上面的trait文件中
* old\(\) - 函数函数 , 通能相同 , 常用在魔棒中`{{ old('name') }}`




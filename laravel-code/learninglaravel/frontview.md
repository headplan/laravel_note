# Front\_View

视图用于向用户呈现网页界面 , 一个文件只要向客户端输出可视内容 , 都成为一个视图 . Laravel应用将视图作为一个独立的组件与控制器解耦 , 所以在任何位置都可以使用view\(\)的方式加载一个视图 . 

> config/view.php是视图的配置文件 , 其中包含两个选项 , 视图文件的家在路径数组 , 和已编译的视图文件存储路径 .

**创建视图**

以字符串形式返回视图文件源码 , 或者以view\(\)方式加载.php , 要使用blade模板语法 , 需要添加.blade.php后缀 . 视图文件的渲染方式也有三种 :

* view\(\)全局函数
* View::make\(\)外观
* view\(\)-&gt;make\(\)对象

第一个参数直接写文件名即可加载 , 子目录可以使用点连接\(目录.文件名\)或者直接写\(目录/文件名\) . 还有其他方法 :

* exists\(\) - 判断视图文件是否存在
* view\(\)-&gt;file\('path'\) - 根据路径加载文件

**传递数据到视图**

* 传递数据view\(\),make\(\),file\(\)的第二个数组参数 , 第三个数组参数还可以传递更多参数 . 
* 使用with\(\)函数传递参数 , 接受两个字符串参数 , 或者第一个数组参数 , 因为返回$this , 支持链式操作 , 多个with\(\)
* 使用with加变量名的方式 , withKey\('value'\) , 同样支持链式操作 . 
* 获取子模板中的内容 , 并填充到指定模板的位置 . 
  * View::make\("hello"\)-&gt;nest\("自定义名称","子模板名称",\['子模板所需的数据'\]\);
* 把数据共享给所有视图 , 可以在服务提供者的boot中使用View::share\(\)方法 , 当然也是支持str和数组两种方式

**视图合成器**

视图合成器是在视图渲染时调用的一些回调或者类方法 . 例如 , 需要在某些视图渲染时绑定一些数据上去 , 或者将这些绑定逻辑整理到特定的位置 .

创建合成器类

```php
<?php
namespace App\Http\ViewComposers;

use Illuminate\View\View;

class ProfileComposer
{
    protected $_num;

    public function __construct()
    {
        $this->_num = 100;
    }

    public function testc(View $view)
    {
        $view->with('tc', $this->_num);
    }

    public function compose(View $view)
    {
        $view->with('count', $this->_num);
    }
}
```

创建合成关联

```php
View::composer('view2', 'App\Http\ViewComposers\ProfileComposer@testc');
```

第一个参数是视图 , 参数可以是str , 也可以是数组 , 将视图合成器附加到多个视图 , 还可以是通配符\* , 应用到所有视图上 . 第二个参数是加载视图时运行的类方法 , 如果没有方法则直接加载类中的compose方法 . 这有点类似我们前面提到的控制器模型绑定 , 这里只要渲染视图 , 就会加载方法 , with需要的数据 .

所有的视图合成器都会由服务容器来解析 , 所以可以在合成器的构造函数中使用类型提示来注入需要的任何依赖 .

**视图构造器**

视图**构造器**和视图合成器非常相似 . 不同之处在于 , 视图构造器在视图实例化时执行 , 而视图合成器在视图渲染时执行 . 这一点在代码中测试就可以看出 , 在return之前打印view对象 , 会执行视图构造器绑定的方法 , 例如我们with了数据 , 就会在打印结果中出现 , 而合成的方式 , 也即是compose的方式就没有数据 .

视图构造器绑定时自动加载的方法是**create\(\)方法** , 当然也可以和视图合成器一样直接绑定指定的方法 , 以@连接 .


# Front\_Blade

Blade 是 Laravel 提供的一个既简单又强大的模板引擎 , Blade 并不限制在视图中使用原生 PHP 代码 , 因为所有 Blade 视图文件都将被编译成原生的 PHP 代码并缓存起来 , Blade 视图必须文件使用`.blade.php`扩展名 , 才能解析其中的blade语法 . 视图和视图编译的缓存文件都在config/view.php中配置 , 默认为resources/views和storage/framework/views路径 .

> 代码实例中的模板套用的是Laravel\_Bootstrap中的模板

### 模板的布局

首先创建一个master布局页面 , 文件保存于`resources/views/layouts/app.blade.php`

* @yield\(\) - 用来显示指定区块的内容 . @yield 是不可扩展的 , 如果要定义的部分没有内容让子模板扩展 , 那么用 @yield\($name, $default\) 的形式会比较方便 , 即在子模板中并没有指定这个区块的内容 , 它就会显示默认内容 , 如果定义了 , 就会显示你定义的内容 . 
* @section\(\) - 用来定义一个视图区块 . 它是可以被替代 , 又可以被扩展的 . 
* * @show - 指的是执行到此处时将该 section 中的内容输出到页面 . 通常首次定义section时 , 应该用show . 但继承或替换的模板中使用show , 表示出现了两次该section , 还会破坏页面结构 . 
  * @stop - 只是进行内容解析 , 并且不再处理当前模板中后续对该section的处理 . 例如继承了一个@stop结尾的section , 则什么也不会显示 . 
  * @overwrite
  * @append
* @extends\(\) - 子页面指定其所**继承**的页面布局 , 也就是说当子页面继承布局之后 , 就可以使用@section命令将内容注入于布局页面中的yield和section中 . 
  * @parent - 追加布局中的section定义的内容 , 使用时和类中的继承类似 , 在section中使用 , 如果不使用则会覆盖掉布局中的这部分内容 . 




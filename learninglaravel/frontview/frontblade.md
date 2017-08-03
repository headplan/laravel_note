# Front\_Blade

Blade 是 Laravel 提供的一个既简单又强大的模板引擎 , Blade 并不限制在视图中使用原生 PHP 代码 , 因为所有 Blade 视图文件都将被编译成原生的 PHP 代码并缓存起来 , Blade 视图必须文件使用`.blade.php`扩展名 , 才能解析其中的blade语法 . 视图和视图编译的缓存文件都在config/view.php中配置 , 默认为resources/views和storage/framework/views路径 .

> 代码实例中的模板套用的是Laravel\_Bootstrap中的模板

### 模板的布局

首先创建一个master布局页面 , 文件保存于`resources/views/layouts/app.blade.php`

* @yield\(\) - 用来显示指定区块的内容 . @yield 是不可扩展的 , 如果要定义的部分没有内容让子模板扩展 , 那么用 @yield\($name, $default\) 的形式会比较方便 , 即在子模板中并没有指定这个区块的内容 , 它就会显示默认内容 , 如果定义了 , 就会显示你定义的内容 . 
* @section\(\) - 用来定义一个视图区块 . 它是可以被替代 , 又可以被扩展的 . 
* * @show - 指的是执行到此处时将该 section 中的内容输出到页面 . 通常首次定义section时 , 应该用show . 但继承或替换的模板中使用show , 表示出现了两次该section , 还会破坏页面结构 . 
  * @stop - 只是进行内容解析 , 并且不再处理当前模板中后续对该section的处理 . 例如继承了一个@stop结尾的section , 则什么也不会显示 . \(@endsection与@stop全等===\)
  * @append - 字面意思追加 , 多个show是显示多个相同的 , 而append解决了这个问题 , 可以追加新的内容到section中 , 使用stop停止后 , 就不能继续追加了 , 当然append之后使用了stop的section中的内容也不显示 . append可以在模板的数据进行遍历输出时使用 . 
  * @overwrite - 字面意思覆盖之前的所有定义 , 以这次的为准 . 它是强制的 , 会覆盖想覆盖的内容 , 不管stop了没有 . 可以用来覆盖掉一些模板中错误的展示 . 
* @extends\(\) - 子页面指定其所**继承**的页面布局 , 也就是说当子页面继承布局之后 , 就可以使用@section命令将内容注入于布局页面中的yield和section中 . 
  * @parent - 追加布局中的section定义的内容\(和section结尾无关联\) , 使用时和类中的继承类似 , 在section中使用 , 如果不使用则会覆盖掉布局中的这部分内容 . 
* @component\(\) - 指令用来加载并构造这个组件 , 其中第一个参数用来指定组件的模板 , 第二个参数可以传递参数到模板当中 . 组件模板中的$slot变量用来接收并渲染component中的构建 , 也可以自定义slot : 
  * @slot\(\) - 其中的参数对应组件模板中的自定义变量
  * @component和@slot都是@end加关键词结尾 , 也就是@endcomponent和@endslot
* @stack - 堆栈 , 一般用在layout中 , 布局的head头引入js文件时 . 
  * @push - 顾名思义 , 往stack里推东西 . 
  * @prepend - 往push之前退东西 , 一般引入js类库时 , 要推倒前面先加载 . \(v5.4.10时才有\)

### 数据的显示

将数据传递给模板前面已经提到过 , 显示变量直接用两个大括号包裹住就可以了 , 也可以显示PHP函数的结果 , 或者任意PHP代码 .

```
The current UNIX timestamp is {{ time() }}.
```

三元运算符也是可以的 , 当然也可以简写

```
{{ $name or 'Default' }}
```

两个大括号包括的数据是经过htmlspecialchars处理的 , 为了防止XSS攻击 , 去掉函数包裹应该这样写

```
Hello, {!! $name !!}.
```

由于很多 JavaScript 框架都使用花括号来表明所提供的表达式 , 可以使用@符抑制blade引擎的渲染 , 显示他原本的样子

```
Hello, @{{ name }}.
```

也可以使用@verbatim指令来包裹大片区块中展示 JavaScript 变量 , 避免了大量的@符抑制

```
@verbatim
    <div class="container">
        Hello, {{ name }}.
    </div>
@endverbatim
```

> blade模板中的注释写法 , 注释经过渲染后不会转成html的注释 , 而是直接被删除
>
> ```
> {{-- 我是注释 --}}
> ```

### 控制结构

**判断**

```
@if(),
@elseif(),
@else,
@endif

@unless(), # 判断为true则不显示
@endunless

@isset(), # 经过料理的判断们
@endisset
@empty(),
@endempty

@hasSection('name') # 判断section存在否
   @yield('name')
@endif

@can() # 判断有么有权限
@elsecan()
@endcan

@cannot()
@elsecannot()
@endcannot

===== # 消失的blade指令

@auth()
@endauth 

@switch
@case 
@default
@endswitch
```

**循环**

```
@for(),
@endfor

@foreach(),
@endforeach

@forelse(),
@empty # 这里的empty是必须的,否则会报错
@endforelse

@while(),
@endwhile

@continue(), # 循环中的两个按钮,括号里写条件
@break()

@unset() # 这只是个函数
```

**循环变量**

循环内访问`$loop`变量 , 可以提供一些有用的信息 :

| 属性 | 描述 |
| :--- | :--- |
| `$loop->index` | 当前循环所迭代的索引，起始为 0。 |
| `$loop->iteration` | 当前迭代数，起始为 1。 |
| `$loop->remaining` | 循环中迭代剩余的数量。 |
| `$loop->count` | 被迭代项的总数量。 |
| `$loop->first` | 当前迭代是否是循环中的首次迭代。 |
| `$loop->last` | 当前迭代是否是循环中的最后一次迭代。 |
| `$loop->depth` | 当前循环的嵌套深度。 |
| `$loop->parent` | 当在嵌套的循环内时，可以访问到父循环中的 $loop 变量。 |

### 引入子模板

```
@include('view.name', ['some' => 'data'])
@includeIf('view.name', ['some' => 'data']) # 没这个视图也没事,不报错
@includeWhen(isset($msg),'default.other',['msg'=>$msg]) # 第一个参数变成了判断条件
@each('default.item',[],'var','default.def') # 组合了include和foreach
- 第一个参数是要include的视图
- 第二个参数是要循环的数据
- 第三个参数是循环数据在引入视图中的变量名
- 第四个参数是如果没有数组是空的没有数据时引入的模板
```

> 视图中的\_\_FILE\_\_和\_\_DIR\_\_会指向渲染后的路径

### 注入服务

从服务容器中取出服务注入到视图中 , 第一个参数是视图中用的变量别名 , 第二个参数是要解析的服务的类或是接口的名称

```
@inject('mymsg', 'App\Http\Controllers\InjectController')
```

### 其他模板指令

```
# 这一对相对简单,在之间输入php代码即可,渲染的也就是<?php ?>标签
@php
...
@endphp


# 这三个都是用来加载resources/lang中的内容的
# 配置文件config/app.php中配置了默认语言和第二默认语言,也就是resources/lang下的文件夹,默认的en就是.
# 第一种情况是使用下面的方式,默认会渲染其之间的内容,其中写的也就是文件.key,渲染成其后的值
# 其调用的是Illuminate\View\Concerns下的ManagesTranslations Trait.其中也就有两个方法
# startTranslation和renderTranslation
@lang
...# 比如写msg.test
@endlang

@lang('msg.test',[],'cn')
# 第二种方式就是单独使用,括号中写参数
# 第一个参数是文件.key
# 第二个参数是lang文件的可变变量用:abc定义.这个参数强制数组
# 第三个参数是使用的语言,就是resources/lang的文件夹名,为空则使用config/app.php配置下的默认值,或者第二默认值

@choice('msg.test',1,[],'cn')
# 这里除了第二个参数,其他和lang的参数一样
# 第二个参数的意思是在渲染的内容中进行选择
    # 参数支持int,就表示选择哪个编号
    # 参数支持arr,这里表示count()后的个数,比如输入一个数组['a','b','c'],选择的编号就是3
    # 参数支持count($arr)函数,这里是SPL的一个对象,也表示最后的个数,可以根据统计个数显示没有,很多,之类的文字.
# 查看Illuminate\Translation下的Translator的choice方法

# 渲染的内容写成下面的样子,就可以实现这个功能了
# 'test'=>'{0}没有|[1,19]一些|[20,*]很多',
# 即第二参数是一个编号,多条数据以竖线分割
# {0}:表示就有一个编号
# [1,19]:表示1到19的数字都可以展示后面的内容
# [20,*]:通配符*,顾名思义,20到无限
# {}和[]都可以
```




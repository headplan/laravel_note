# Blade模板引擎

Blade 视图文件使用`.blade.php`文件扩展并存放在`resources/views`目录下.并解释成原生PHP缓存在下面的目录下`storage/framework/views/`

### 变量输出

通过两个花括号包裹变量来显示传递到视图的数据,还可以输出任何 PHP 函数,实际上可以将任何 PHP 代码放到 Blade 模板语句中.

```
<div class="title">{{ $title }}</div>
The current UNIX timestamp is {{ time() }}.
```

> 注意:Blade的`{{}}`语句已经经过PHP的`htmlentities`函数处理以避免XSS攻击.

屏蔽掉`{{}}`解析方式,可以在前面添加`@`符号\(一些JS框架也会用到花括号,例如AngularJS\).

```
The current UNIX timestamp is @{{ time() }}.
```

如果像上面一样排除`htmlentities`函数的处理,比如输出`<script>`标签,会直接被过滤掉,可以使用下面的方式原生输出

```
{!! $title !!}
# 假设控制器传过来的变量是html代码
$title = '<script>document.write("我是HTML")</script>';
```

不确定该变量是否被设置,可以给其一个默认值,通常可以使用PHP原生的三元运算,Blade模板引擎提供了`or`的方式

```
{{ isset($name) ? $name : 'Default' }}
{{ $name or 'Default' }}
```

> isset:0,'0','',都显示,null,false不显示.
> 变量:0,'0','',false,null,都不显示.
> empty:0,'0','',false,null,都是空.
> 变量和empty本质上没有区别,`if($var)`如果变量不存在会警告,`if(!empty($var))`不会警告.
> 如果应用在\_\_get之类的魔术方法上,empty一定会判断为空.

### 流程控制

**if语句**

可以使用 `@if` , `@elseif` , `@else` 和 `@endif` 来构造 if 语句,这些指令函数和 PHP 的相同:

```
<p>
    @if ($data['score'] < 60 )
        没及格
    @elseif ($data['score'] > 70)
        我很优秀
    @else
        我及格了
    @endif
</p>
```

为了方便,Blade还提供了`@unless`指令,是除非的意思

```
@unless ($data['score'] > 60)
    不及格
@endunless
```

**循环语句**

for循环

```
<p>
    @for ($i = 0; $i <= $data['num']; $i++)
        {{ $i }}
    @endfor
</p>
```

foreach循环

```
<p>
    @foreach ($data as $k => $v)
        {{ $k }}{{ $v }}
    @endforeach
</p>
```

forelse循环,如果循环中没有数据,显示指定内容

```
<p>
    @forelse ($data as $k => $v)
        {{ $k }}{{ $v }}
        @empty
        没有数据
    @endforelse
</p>
```

while循环

```
@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

### 模板继承与布局

**定义布局**

Web应用的页面布局基本相同,所以可以定义一个布局页面,将布局定义为一个单独的blade页面

相关指令:

* @yield\('content'\) - 指定layout模板模型中改变的部分
* @extends\('layout.master'\) - 继承模板模型
* @section\(\)
* @show\(\)
* @endsection\(\)
* @parent\(\)

**包含子视图**

common是文件夹名,在resources\/views\/common下,@include\(\)第二个参数可以向子视图中传递参数

```
@include('common.header', ['title' => '我是首页'])
<div class="middle">我是中间</div>
@include('common.footer')
```

> 在blade视图中使用类似\_\_DIR\_\_和\_\_FILE\_\_常量,会指向缓存路径storage\/framework\/views

**循环引入多个局部视图**

使用blade的@each指令,其有4个参数:

**注释**

```
{{-- 我是blade注释 --}}
```

在blade中使用注释,不会渲染到HTML中,因为缓存文件渲染成了php注释.

```
<?php /* 我是blade注释 */ ?>
```


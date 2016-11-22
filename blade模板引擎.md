# Blade模板引擎

通过两个花括号包裹变量来显示传递到视图的数据,还可以输出任何 PHP 函数,实际上可以将任何 PHP 代码放到 Blade 模板语句中.

```
<div class="title">{{ $title }}</div>
The current UNIX timestamp is {{ time() }}.
```

> 注意:Blade的`{{}}`语句已经经过PHP的`htmlentities`函数处理以避免XSS攻击.

屏蔽掉`{{}}`解析方式,可以在前面添加`@`符号\(一些JS框架也会用到花括号,例如angular\)

```

```


# HTTP视图

创建视图文件`view.blade.php`在`/resources/views`目录,然后创建控制器和路由,再控制器中引入视图文件

```
return view('view');
```

> 上一小结中使用了redirect\(\)重定向函数,其也需要return返回,否则无效.

**视图传参**

使用with\(\)函数传参,两个参数,可以连续多次使用with\(\)函数

```
return view('myview')->with('val', $val)->with('name', $name);
```

也可以直接使用view\(\)函数第二个参数,直接将数组传过去

```
$data = [
    'val' => '[我是视图]',
    'name' => '第二个参数'
];
return view('myview', $data);
```

更好的传参方式,compact\(\)函数,这是一个PHP内置函数:

```
$data = [
    'val' => '[我是视图]',
    'name' => '第二个参数'
];
$title = '我是另一个参数';
return view('myview', compact('data', 'title'));
```


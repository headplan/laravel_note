# Simditor编辑器

```
$ npm install simditor
$ bower install simditor
```

也可以下载 :

```
https://github.com/mycolorway/simditor/releases
```

引入文件 :

```
<link rel="stylesheet" type="text/css" href="[style path]/simditor.css" />

<script type="text/javascript" src="[script path]/jquery.min.js"></script>
<script type="text/javascript" src="[script path]/module.js"></script>
<script type="text/javascript" src="[script path]/hotkeys.js"></script>
<script type="text/javascript" src="[script path]/uploader.js"></script>
<script type="text/javascript" src="[script path]/simditor.js"></script>
```

* Simditor基于jQuery和module. js
* hotkeys.js是用来绑定热键的
* uploader.js与上传文件有关 . 如果不需要上载功能 , 则无需导入此文件

在项目中使用 :

```
import Module from 'simple-module';
import Hotkeys from 'simple-module';
import Uploader from 'simple-uploader';
import Simditor from 'simditor';

var editor = new Simditor({
    textarea: $('#editor')
    //optional options
});
```

```
@import "~Simditor/styles/simditor";
```

自定义样式也可以修改这个文件 , 编辑器里文字 , 字体的样式可以修改`.editor-style`

#### 添加扩展

**官方扩展**

* [simditor-emoji](https://github.com/mycolorway/simditor-emoji) : 在内容中插入 Emoji 图片
* [simditor-mention](https://github.com/mycolorway/simditor-mention) : Simditor的@人扩展
* [simditor-autosave](https://github.com/mycolorway/simditor-autosave) : 运用 HTML5 的 localStorage 技术来自动保存用户输入内容
* [simditor-mark](https://github.com/mycolorway/simditor-mark) : 为工具栏添加一个荧光笔按钮 , 用于高亮所选内容
* [simditor-html](https://github.com/mycolorway/simditor-html) : 为 Simditor 添加 HTML 源代码编辑按钮
* [simditor-markdown](https://github.com/mycolorway/simditor-markdown) : 为 Simditor 添加一个markdown编辑按钮
* [simditor-checklist](https://github.com/mycolorway/simditor-checklist) : 为工具栏添加一个 checklist 按钮 , 用于插入 checklist

**第三方扩展**

* [simditor-marked](https://github.com/huyinghuan/simditor-marked) : 格式化编辑器里面markdown的内容
* [simditor-dropzone](https://github.com/ichord/simditor-dropzone) : 拖拽上传插入图片
* [simditor-fullscreen](https://github.com/heroicyang/simditor-fullscreen) : 编辑器全屏支持

#### 初始化配置

    var editor = new Simditor({
        textarea: $('#editor'),
        placeholder: '请使用 Markdown 格式书写(′▽`〃),代码片段黏贴时请注意使用高亮语法.',
        toolbar: true,
        //optional options
    });  

textarea : 在id上创建编辑器 . 默认null

placeholder : 占位符 , textarea为空时显示的文字. 默认 , ''

toolbar : 工具栏显示按钮 . 默认 true

```
[
  'title'
  'bold'
  'italic'
  'underline'
  'strikethrough'
  'fontScale'
  'color'
  'ol'             # ordered list
  'ul'             # unordered list
  'blockquote'
  'code'           # code block
  'table'
  'link'
  'image'
  'hr'             # horizontal ruler
  'indent'
  'outdent'
  'alignment'
]
```

toolbarFloat - 修复滚动浏览器时工具栏在其顶部的问题 , 默认true .

toolbarFloatOffset - 固定工具栏的顶部偏移量 , 默认0.

toolbarHidden - 隐藏工具栏 . 无法和`toolbarFloat`一起使用 . 默认false

defaultImage - 默认图像占位符 . 在 Simditor 中插入图片时使用 . 默认images/image.png

tabIndent - 使用tab缩进 , 默认true

params - 添加隐藏域 , 写key:value , 默认{}

upload - 上传图片的额外选项 , 默认false

```
url - 处理上传图片的URL.
params - 表单提交的参数,Laravel的POST请求必须带防止CSRF跨站请求伪造的_token参数,例如{ _token: '{{ csrf_token() }}' }
fileKey - 是服务器端获取图片的键值,设置upload_file
connectionCount - 最多同时上传图片限制,设置3
leaveConfirm - 上传过程中,用户关闭页面时的提醒.比如'图片上传中,关闭此页面将取消上传.'
```

上传完成后的json响应 :

```
{
  "success": true/false,
  "msg": "error message", # optional
  "file_path": "[real file path]"
}
```

pasteImage - 设定是否支持图片黏贴上传,可以设置true开启 .

cleanPaste - 自动删除粘贴内容中的所有样式 .

imageButton - 开启从本地或者外部链接插入图片 , 默认两者都开启 , 图像按钮显示下拉菜单 . 默认值:\['upload', 'external'\]

allowedTags - 标签白名单 , 自定义的也是追加合并的 , 所以默认null . 默认的标签有 :

```
['br', 'span', 'a', 'img', 'b', 'strong', 'i', 'strike', 'u', 'font', 'p', 'ul', 'ol', 'li', 'blockquote', 'pre', 'code', 'h1', 'h2', 'h3', 'h4', 'hr']
```

allowedAttributes - 属性白名单 , 自定义的也是追加合并的 , 所有默认null . 默认的属性有 :

```
img: ['src', 'alt', 'width', 'height', 'data-non-image']
a: ['href', 'target']
font: ['color']
code: ['class']
```

allowedStyles - 内嵌样式白名单 , 同上 :

```
span: ['color', 'font-size']
b: ['color']
i: ['color']
strong: ['color']
strike: ['color']
u: ['color']
p: ['margin-left', 'text-align']
h1: ['margin-left', 'text-align']
h2: ['margin-left', 'text-align']
h3: ['margin-left', 'text-align']
h4: ['margin-left', 'text-align']
```

codeLanguages - 代码块支持的编程语言列表 :

```
[
  { name: 'Bash', value: 'bash' }
  { name: 'C++', value: 'c++' }
  { name: 'C#', value: 'cs' }
  { name: 'CSS', value: 'css' }
  { name: 'Erlang', value: 'erlang' }
  { name: 'Less', value: 'less' }
  { name: 'Sass', value: 'sass' }
  { name: 'Diff', value: 'diff' }
  { name: 'CoffeeScript', value: 'coffeescript' }
  { name: 'HTML,XML', value: 'html' }
  { name: 'JSON', value: 'json' }
  { name: 'Java', value: 'java' }
  { name: 'JavaScript', value: 'js' }
  { name: 'Markdown', value: 'markdown' }
  { name: 'Objective C', value: 'oc' }
  { name: 'PHP', value: 'php' }
  { name: 'Perl', value: 'parl' }
  { name: 'Python', value: 'python' }
  { name: 'Ruby', value: 'ruby' }
  { name: 'SQL', value: 'sql'}
]
```




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

[toolbarFloat](http://simditor.tower.im//docs/doc-config.html#anchor-toolbarFloat) - 修复滚动浏览器时工具栏在其顶部的问题 , 默认true . 




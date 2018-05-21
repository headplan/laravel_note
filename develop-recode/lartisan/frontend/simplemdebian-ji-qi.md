# SimpleMDE编辑器

```
https://simplemde.com/
https://github.com/sparksuite/simplemde-markdown-editor
```

#### 安装

```
yarn add simplemde --save
```

#### 配置参数

**autoDownloadFontAwesome** : 如果设置为 true , 强制下载Font Awesome字体 \(用于图标\) . 如果设置为 false , 则禁止下载 . 默认为未定义 , 它将智能地检查是否已包含Font Awesome字体 , 然后下载相应的 . 

**autofocus** : 自动对焦 . 默认false . 

**autosave** : 自动保存 . 保存正在写入的文本 , 并在将来将其加载回来 . 保存提交表单时就没了 . 

* **enabled** : 开启自动保存 , 默认为false . 
* **delay** : 保存之间的延迟\(以毫秒为单位\) . 默认为 10000\(10s\) . 
* **uniqueId** : 必须设置唯一的字符串标识符 , 以便 SimpleMDE 可以自动保存 . 与您网站上其他 SimpleMDE 的其他实例分离的东西 . 

**blockStyles** : 自定义样式文本块的某些按钮的行为方式 . 

* **bold** 可以设置 `**` or `__` . 默认 `**` .
* **code** 可以设置 ``````````` or `~~~`. 默认 ```````````.
* **italic** 可以设置 `*` or `_` . 默认 `*` .

**element** : 要使用的 textarea 的 DOM 元素 . 默认为页面上的第一个 textarea . 

```
element: document.getElementById("MyID"),
```

**forceSync** : 当内容改变时 , 是否进行强制存储 , 默认值false . 

**hideIcons** : 要隐藏的图标名称数组 . 可以用于隐藏默认显示的图标 , 而不完全自定义工具栏 . 

**indentWithTabs** : 为false时 , 缩进使用空格而不是制表符 , 默认值true . 

**initialValue** : 如果设置了该项 , 则为editor设置了默认值 . 

**insertTexts : **设置插入的内容 , 值为一个包含两个元素的数组 , 第一个元素将是在光标或高亮之前插入的文本 , 第二个元素将插入到后面 . 这是默认链接值：`["[", "](http://)"]`

* horizontalRule
* image
* link
* table

**lineWrapping** : 为false时禁用换行 , 默认值true . 

**parsingConfig** : 调整在编辑期间分析Markdown的设置\(不是预览\) . 

* **allowAtxHeaderWithoutSpace **如果设置为 true , 将在 \# 之后呈现没有空格的标题 . 默认为 false . 
* **strikethrough **如果设置为 false , 则不会处理 GFM 删除线语法 . 默认为 true . 
* **underscoresBreakWords **如果设置为 true , 则让下划线成为分隔单词的分隔符 . 默认为 false . 

**placeholder** : 显示自定义占位符 . 

**previewRender** : 定义解析纯文本Markdown和返回HTML时的自定义函数 , 在用户预览时使用 . 

**promptURLs** : 如果设置为 true , 则会出现一个 JS 警告窗口 , 要求链接或图像 URL . 默认为 false . 

**renderingConfig** : 定义预览Markdown时的设置 . 

* **singleLineBreaks** 为false时禁用GFM的单行中断 , 默认true .
* **strikethrough** 是否使用[highlight.js](https://github.com/isagalaev/highlight.js)高亮插件 , 默认false . 注意 , 为true是你需要引入相关文件 . 

```
<script src="https://cdn.jsdelivr.net/highlight.js/latest/highlight.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/highlight.js/latest/styles/github.min.css">
```

**shortcuts** : 设置键盘快捷键 , 默认为下面的列表 . 

**showIcons** : 要显示的图标名称数组 . 可用于显示默认情况下隐藏的特定图标 , 而不完全自定义工具栏 . 

**spellChecker : **是否启用拼写检查 , 默认值true . 

**status** : 如果设置为 false , 则隐藏状态栏 . 默认为内置状态栏项的数组 . 

* 或者 , 可以将状态栏项的数组设置为包含 , 并按顺序排列 . 您甚至可以定义自己的自定义状态栏项 . 

**styleSelectedText** : 如果设置为 false , 将从所选行中移除 CodeMirror selectedtext 样式 . 默认为true . 

**tabSize** : 如果设置 , 则自定义制表符大小 . 默认为2 . 


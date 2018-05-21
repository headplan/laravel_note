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






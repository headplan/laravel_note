# HTTP\_Cookie\_N\_File

#### **File资源**

请求和响应部分的整理 . 

**获取上传文件**

使用`Illuminate\Http\Request`实例中的`file`方法获取上传的文件 . file 方法返回的对象是`Symfony\Component\HttpFoundation\File\UploadedFile`类的实例 , 该类继承了 PHP 的`SplFileInfo`类 , 并提供了许多和文件交互的方法 : 

* $request-&gt;file\('photo'\);或$request-&gt;photo;
* $request-&gt;hasFile\('photo'\) - 判断上传文件是否存在
* $request-&gt;file\('photo'\)-&gt;isValid\(\) - 验证上传的文件是否有效
* $request-&gt;photo-&gt;path\(\); - 访问文件完整路径
* $request-&gt;photo-&gt;extension\(\); - 根据文件内容猜测文件的扩展名

**存储上传文件**

设置文件系统的配置信息 , 把上传文件储存到本地磁盘或者云上

* $request-&gt;photo-&gt;store\('images', 's3'\); - 第一个参数是相对于文件系统根目录配置的路径 , 第二个可选参数是存储到磁盘的名称
* $request-&gt;photo-&gt;storeAs\('images', 'filename.jpg', 's3'\); - 第二个参数是设置文件名 , 其他两个参数同上

**其他上传文件方法API**

http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/File/UploadedFile.html


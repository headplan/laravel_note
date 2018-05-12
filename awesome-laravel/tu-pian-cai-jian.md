# 图片裁剪

[https://github.com/Intervention/image](https://github.com/Intervention/image)

```
# 安装
composer require intervention/image
# 生成配置
php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravel5"
```

#### 配置文件

```
'driver' => 'gd'
```

默认使用gd库 , 还可以使用imagick库 .

```
// open an image file
$img = Image::make('public/foo.jpg');

// resize image instance
$img->resize(320, 240);

// insert a watermark 插入水印
$img->insert('public/watermark.png');

// save image in desired format
$img->save('public/bar.jpg');
```

```
安装：
	需求：
		PHP >= 5.4
		Fileinfo 扩展
		GD库 >= 2.0
		Imagick 扩展 >=6.5.7
	composer安装：
		composer require intervention/image
	laravel配置：
		1.编辑 config/app.php
			$providers 添加 'Intervention\Image\ImageServiceProvider::class'
			$aliases 添加 ''Image' => Intervention\Image\Facades\Image::class'
		2.默认使用的是 'GD' 库，想修改的话，需要配置驱动，我们来生成配置文件：
			php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravel5"
			生成 config/image.php 配置文件
		3.PHP涉及到的配置项：
			memory_limit - 最大内存限制
			upload_max_filesize - 如果上传图片时，可能修改
			－－－－－
			其实，可能还有好多
			max_execution_time - 最大执行时间
			...

基本使用：
	1.使用 Intervention\Image\ImageManager - 图片管理类
		use Intervention\Image\ImageManager;
		$manager = new ImageManager(array('driver' => 'imagick'));
		$manager->make('public/foo.jpg');
	2.使用 ImageManager 的静态版本
		use Intervention\Image\ImageManagerStatic as Image;
		Image::configure(array('driver' => 'imagick'));		------- api中未出现此方法
		Image::make('public/foo.jpg');	
	
HTTP响应：
	1.将图像直接返回到用户浏览器的最简单的方法是输出response（）方法。 它将根据当前图像自动发送HTTP头，并输出编码的图像数据。
		$img = Image::canvas(800, 600, '#ff0000');
		echo $img->response('jpg', 70);
	2.手动发送HTTP响应
		$img = Image::canvas(800, 600, '#ff0000');
		header('Content-Type: image/png');
		echo $img->encode('png');

	在laravel中，同上面的2种方法一样：

	3.
		$img = Image::canvas(800, 600, '#ff0000');
		return $img->response();
	4.
		$img = Image::canvas(800, 600, '#ff0000');
		$response = Response::make($img->encode('png'));
		$response->header('Content-Type', 'image/png');
		return $response;
	
图片上传：
	1.直接从 $_FILES 中，获取 'tmp' 临时图片数据
		$img = Image::make($_FILES['image']['tmp_name']);		// Image::make() 支持这种方式
		$img->fit(300, 200);
		$img->save('public/bar.jpg');
	2.在laravel中处理图片上传
		Image::make(Input::file('photo'));						// Input::file() 来获取$_FILES

图片过滤器：
	图片过滤器，给了我们非常有用的方式，将多个图像转换命令集合在一个专用类中。(可将多个图像处理步骤，封装成一个过滤器)。
	Intervention/image，提供了基本接口(Intervention\Image\Filters\FilterInterface)，所有的过滤器都需要实现它。
	调用过滤器：
		$img = Image::make('foo.jpg');
		$img->filter(new DemoFilter(5));
	定义过滤器：
		src/Intervention/Image/Filters/DemoFilter.php
		<?php
			namespace Intervention\Image\Filters;

			class DemoFilter implements FilterInterface
			{
			    /**
			     * Default size of filter effects
			     */
			    const DEFAULT_SIZE = 10;

			    /**
			     * Size of filter effects
			     *
			     * @var integer
			     */
			    private $size;

			    /**
			     * Creates new instance of filter
			     *
			     * @param integer $size
			     */
			    public function __construct($size = null)
			    {
			        $this->size = is_numeric($size) ? intval($size) : self::DEFAULT_SIZE;
			    }

			    /**
			     * Applies filter effects to given image
			     *
			     * @param  Intervention\Image\Image $image
			     * @return Intervention\Image\Image
			     */
			    public function applyFilter(\Intervention\Image\Image $image)
			    {
			        $image->pixelate($this->size);
			        $image->greyscale();

			        return $image;
			    }
			}
		?>

图像缓存：
	图像缓存包，扩展了缓存图像的能力。

	缓存包使用laravel的 'Illuminate/Cache' 包。基于laravel的缓存配置，可以使用 '文件系统'、'数据库'、'Memcached'或'Redis' 来作为临时缓冲存储

	原理很简单。一旦安装了缓存包，就可以调用静态缓存方法。每个对 Intervention/Image 类的方法调用，都会被缓存接口捕获和检查。如果这个特定的操作序列意境生成过，将直接从缓存中获取数据，而不是重新执行一遍资源密集型的GD操作

	安装：
		composer require intervention/imagecache

	使用方式：
		$img = Image::cache(function($image){
			$image->make('public/foo.jpg')->resize(300, 200)->greyscale();
		});

基于URL的图像操作：
	在Laravel应用程序中，可以使用URL来动态操作图像。 URL图像的操作版本将存储在缓存中，并且将直接加载而无需资源密集型GD操作。

	图像必须上传一次。当通过HTTP请求方式访问文件，所有操作(例如：调整大小、裁剪)将稍后处理，例如：
		http://yourhost.com/{route-name}/{template-name}/{file-name}

	1.安装：
		composer require intervention/image
		composer require intervention/imagecache
	2.生成配置文件
		php artisan vendor:publish
		生成 config/imagecache.php
	3.启用操作
		默认情况下，基于URL的图像操作是关闭的。在imagecache.php配置文件中，来指定 'route' 配置项
			'route' => 'imagecache'	
		配置好route后，可以通过artisan命令，列出所有已注册的路有，并检查新路有是否正确列出：
		  	php artisan route:list
	4.定义资源目录
		告诉PHP，在哪里搜索图片。我们可以定义自己喜欢的任意数量的目录。例如：定义应用程序的所有上传目录。应用程序将在目录中搜索在路有中提交的文件。
			'paths' => array(
				'storage/uploads/images',
				public_path('img'),
			),
		在这些目录中保存具有唯一文件名的图像文件是有意义的。否则，包将返回首先找到的图像。
		注意：	
			出于安全原因，路由仅接受由以下字符组成的文件名：
				a-zA-Z0-9-_/.
	5.模板
		模板的定义，就是过滤器类的名称，可以在其中定义任意操作命令(之前介绍过过滤器)。系统自带3个基本模板：
			small - 120x90 
			medium - 240x180
			large - 480x360
		我们可以在配置文件中，自由配置可用的模板：
			'templates' => array(
				'small' => 'Intervention\Image\Templates\Small',
				'medium' => 'Intervention\Image\Templates\Medium',
				'large' => 'Intervention\Image\Templates\Large',
			),
		访问原始图片：
			original - 使用原始图像文件，发送HTTP响应
			download - 发送HTTP响应并强制浏览器下载原始图像文件，而不是显示它。
			例如：
				http://yourhost.com/{route-name}/original/{file-name}
	6.图像缓存的生命周期：
		一旦第一次访问了URL，搜索图片，并根据模板，编辑图像，并存储到缓存中。 所以下次访问URL时，所有的GD操作都被绕过，文件将直接来自缓存
			'lifetime' => 43200,		// 单位是 'minutes-分钟'


支持的格式：
	图片格式：
		GD库：jpeg, png, gif
		Imagick库：jpeg, png, gif, tif, bmp, ico, psd
	颜色格式：
		整数
			$color = Image::make('public/foo.jpg')->pickColor(10, 10);	// pickColor()返回的是整数形式的RGB值
			$img->fill($color);
		数组
			$img->fill(array(255, 0, 0));
			$img->fill(array(255, 0, 0, 0.5));
		16进制
			$img->fill('#000000');
		RGB和RGBA字符串格式
			$img->fill('rgb(255, 0, 0)');
			$img->fill('rgb(255, 0, 0, 0.5)');

创建图像：
	1.canvas($width, $height [, mixed $bgcolor]) - 创建一个空的画布

		构造方法，使用给定的宽、高，创建一个新的空的图像实例。可以选择性的定义一个背景色。默认画布背景是透明(transparent)的。

		示例：
			$img = Image::canvas(800, 600);
			$img = Image::canvas(32, 32, '#ff0000');

	2.make($source) - 从给定的资料中读取图像

		从资源中创建新的图像实例的通用工厂方法。该方法高度可变，可读取下面列出的所有输入类型:
			string - 文件系统的图片路径
			string - 图片的URL地址(allow_url_fopen必须启用)
			string - 二进制图片数据
			string - data-url编码的图片数据
			string - base64编码的图片数据
			resource - gd类型的PHP资源(当使用GD库)
			object - Imagick实例(当使用Imagick库)
			object - Intervention\Image\Image 实例
			object - SplFileInfo instance (To handle Laravel file uploads via Symfony\Component\HttpFoundation\File\UploadedFile) - laravel框架自带的图片上传实例

销毁图像：
	1.destroy() - 销毁图像

		在PHP脚本结束前，释放当前图像实例相关的内存。通常资源会在脚本结束后，自动释放。
		当然，该方法调用后，图像实例不再可用。

		示例：
			$img = Image::make('public/foo.jpg');
			$img->resize(320, 240);
			$img->save('public/small.jpg');
			$img->destroy();

图像输出：
	1.response([string $format [, integer $quality]]) - 直接作为HTTP响应

		以指定的格式和图像质量，来发送当前图像，作为HTTP响应
		$format - 可以是:jpg,png,gif,tif,bmp。默认是jpeg
		$quality - 从0-100.默认是90

	2.save([string $path [, int $quality]]) - 保存图像到指定路径(未指定，则表示覆盖原图)

		将图像对象的当前状态保存到文件系统中。可指定路径和图像质量。
		保存到图像类型通过文件后缀来定义。例如：foo.jpg 图片将被保存为 'JPG' 格式。如果未指定可用后缀，首先尝试图像的MIME类型，如果失败，将被编码为 'JPEG'
		此外，图像将始终以RGB颜色模式保存，而没有嵌入的颜色配置文件。
		$path - 指定图像数据写入的文件路径。如果图像是从一个存在的文件路径创建的，同时我们未指定 $path，将会尝试覆盖该路径。

		示例：
			$img = Image::make('public/foo.jpg')->resize(300, 200);
			$img->save('public/bar.png', 60);
			$img->save('public/bar.jpg');

	3.stream([mixed $format [, int $quality]]) - 图像流

		以给定格式和给定图像质量，对当前图像进行编码，并基于图像数据，创建新的PSR-7流。

	4.encode([$format [, $quality]]) - 图像进行编码

		将当前图片，按给定的格式和图片质量进行编码

		$format - jpg、png、gif、tif、bmp、data-url(base64)。默认返回的编码后的数据。默认类型是 'jpeg'
		$quality - 返回从 0-100。0-最低，100最大。只对 'jpg' 编码格式有效，因为png压缩是无损的，不会影响图片质量。默认是90

		示例：
			$jpg = (string) Image::make('public/foo.png')->encode('jpg', 75);
			$data = (string) Image::make('public/foo.png')->encode('data-url');

图像缓存相关
	1.cache(Closure $callback [, int $lifetime [,bool $returnObj]]) - 图像缓存

		$callback - 从Closure回调，创建新的缓存图像实例。
		$lifetime - 为回调传递一个生命周期
		$returnObj - 获取一个Intervention图像实例作为返回值，还是直接接收图像流。
		cache()方法，需要安装额外的 'intervention/imagecache' 依赖包

		示例：
			$img = Image::cache(function($image){
				$image->make('public/foo.jpg')->resize(300, 200)->greyscale();
			}, 10, true);

	2.backup($name = 'default') - 图像状态备份

		备份当前图片的状态。将图片的当前状态备份到 $name。之后，可以调用 reset($name = 'default') 来进行状态恢复。

		$name默认是'default'。我们可以传递不同的$name，来记录图片处理过程中的多个状态

		示例：
			$img = Image::canvas(120, 90, '#000');
			$img->fill('#f00');
			$img->backup();
			$img->fill('#0f0');
			$img->reset();

	3.reset($name = 'default') - 图像状态恢复

		backup()用于备份图像状态，reset()用于恢复到之前的某个备份	

原生图像处理驱动调用：
	1.getCore() - GD库｜Imagick库 对象调用

		以特定驱动的核心格式，返回当前图像。如果使用的是 'GD' 库，则返回 GD 资源类型；如果使用的是 'Imagick'，则返回一个 Imagick 对象。

		示例：
			$img = Image::make('public/foo.jpg');
			$imagick = $img->getCore();			// 返回原始的 'imagick' 对象
			$imagick->embossImage(0, 1);		// 就可以调用 'imagick' 的方法了

调整图像尺寸：
	1.resize($width, $height [,Closure $callback]) - 调整图像尺寸

		使用给定的宽、高，来调整当前图像。传递一个可选的Closure回调来约束resize命令。
		回调函数，定义了resize命令的约束。可约束图像的 '宽高比(aspect-ratio)' 和 '不希望的大小(a unwanted upsizing)'
			aspectRatio()
				约束当前图像的宽高比。作为比例调整大小的快捷方式，可以使用 widen() 和 heighten()
			upsize()
				保持图像大小

		示例：
			$img = Image::make('public/foo.jpg');
			$img->resize(300, 200);
			$img->resize(300, null);
			$img->resize(null, 200);
			$img->resize(300, null, function($constraint){		// 调整图像的宽到300，并约束宽高比(高自动)
				$constraint->aspectRatio();
			});
			$img->resize(null, 200, function($constraint){		// 调整图像的高到200，并约束宽高比(宽自动)
				$constraint->aspectRatio();
			});
			$img->resize(null, 400, function($constraint){		// 阻止可能的尺寸变化(保持图像大小)
				$constraint->aspectRatio();
				$constraint->upsize();
			});

	2.widen($width [, Closure $callback]) - 调整图像宽(保持宽高比)

		调整图像的宽到给定的宽度，保持宽高比。传递一个可选的回调函数，来应用额外的约束，例如：阻止可能的尺寸变化
			upsize()
				保持图像大小

	3.heighten($height [, Closure $callback]) - 调整图像高(保持宽高比)

		调整图像的高到给定的高度，保持宽高比。传递一个可选的回调函数，来应用额外的约束，例如：阻止可能的尺寸变化
			upsize()
				保持图像大小

	4.crop($width, $height [, $x, $y]) - 裁剪图像

		使用给定的宽、高，裁剪当前图像的一个矩形区域。默认从图像的0.0开始，可传递一个坐标，定位裁剪的起始点。

		示例：	
			$img = Image::make('public/foo.jpg');
			$img->crop(100, 100, 20, 20);

	5.fit($width [[$height] [, Closure $callback [, $position]]]) - 智能裁剪和调整

		以一个智能的方式，结合裁剪和调整来格式化图片。该方法将会自动找到给定的宽、高的最佳宽高比，裁剪并调整到给定尺寸。
		$callback - 回调函数，来约束可能的尺寸变化
			upsize()
				保持图像大小

		$position - 自定义裁剪的位置。默认是 '中心'
			top-left
			top
			top-right
			left
			center(默认)
			right
			bottom-left
			bottom
			bottom-right

	6.resizeCanvas($width, $height [, $anchor [, bool $relative [, $bgcolor]]])

		调整当前图像的边界到给定的宽和高。

		$anchor - 从图像的哪点开始调整
			top-left
			top
			top-right
			left
			center(默认)
			right
			bottom-left
			bottom
			bottom-right

		$relative - 将模式设置为相对，用以在真实的图片尺寸上，添加或者减去给定的宽、高。

		$bgcolor - 画布背景(默认 '#000000')

	7.trim([$base [, array $away [, $tolerance [, $feather]]]])
		以给定颜色修剪图像空间。

		$base - 定义从什么位置来选取修剪颜色的点。例如：设置为  'bottom-right'，图像上的所有颜色将被修剪掉，等同于图片左下角的颜色。可选的值有：
			top-left(默认)
			bottom-right
			tranparent

		$away - 应该被修剪掉的边框。你可以添加多个边框(array)，可选的值有：
			top
			bottom
			left
			right

		$tolerance - 定义一个偏差度，修剪相似颜色值。范围0-100。默认是0

		$feather - 羽化效果
			有时，在修剪时在对象周围留下未触摸的“边框”可能是有用的。 特别是修剪非实体背景时，您可以展开（正值）或收缩（负值）修剪对象周围的空间一定量的像素。

		示例：
			Image::make('public/foo.jpg')->trim();		// 默认，所有边框使用 'top-left' 颜色
			Image::make('public/foo.jpg')->trim('bottom-right');		// 所有边框使用 'bottom-right' 颜色
			Image::make('public/foo.jpg')->trim('transparent', array('top', 'bottom'));		// 上、下边框透明
			Image::make('public/foo.jpg')->trim('top-left', 'left');		// 只有左边框，使用 'top-left' 颜色
			Image::make('public/foo.jpg')->trim('top-left', null, 40);		// 移除图像所有边框＋40偏差度
			Image::make('public/foo.jpg')->trim('top-left', null, 25, 50);		// 移除图像所有边框＋25偏差度＋50px的边框羽化效果


调整图像：
	1.gamma($correction) - 伽马矫正

		对当前图像执行伽马矫正操作
		$correction - 伽马补偿值(gamma compensation value)

		示例：
			$img = Image::make('public/foo.jpg');
			$img->gamma(1.6);

	2.brightness($level) - 亮度

		改变当前图像的亮度。$level可选范围为：-100 - 100。0-表示不改变，-100最小，＋100最大。

		示例：
			$img = Image::make('public/foo.jpg');
			$img->brightness(35);

	3.constrast($level) - 对比度

		改变图像的对比度。$level可选范围为：-100 - 100。0-表示不改变，-100最小，＋100最大。

		示例：
			$img = Image::make('public/foo.jpg');
			$img->constrast(35);

	4.colorize($red, $green, $blue) - 改变RGB色道

		使用给定的红、绿、蓝色道，改变当前图像的RGB颜色。色道值是规范化的，范围从 -100 - 100。0-表示不改变，－100表示移除图像上的所有特定颜色，＋100表示最大颜色

		示例：
			$img = Image::make('public/foo.jpg');
			$img->colorize(-100, 0, 100);	// 添加蓝，移除红
			$img->colorize(0, 30, 0);		// 给图片添加部分蓝色调

	5.greyscale() - 灰度

		将图像转换为灰度版本

		示例：
			$img = Image::make('public/foo.jpg');
			$image = $img->greyscale();

	6.flip($mode) - 图像镜像

		给当前图像，制作镜像。$mode可以指定为：h-水平镜像和v-垂直镜像。默认是h

		示例：
			$img = Image::make('public/foo.jpg');
			$img->flip('v');

	7.invert() - 反转图像颜色

		反转当前图像的所有颜色

		示例：
			$img = Image::make('public/foo.jpg')->invert();

	8.opacity($transparency) - 设置图像的透明度

		设置当前图像的不透明度的百分比。从 100%-0%，100%－不透明｜0%-全透明

		示例：
			Image::make('public/foo.jpg')->opacity(50);
			Image::make('public/foo.jpg')->opacity(0);

	9.orientate() - 旋转图像(记得摩点图像处理，使用了它。Imagick)

		此方法读取EXIF图像配置项 '方向'，并对图像执行旋转，以正确显示图像。必须从文件路径实例化图像，才能正确读取EXIF数据
		注意：
			使用该方法，要求PHP编译时，必须指定 '--enable-exif'。windows用户，还必须启用 'mbstring' 扩展

		示例：
			$img = Image::make('public/foo.jpg')->orientate();

	10.rotate(float $angle [, string $bgcolor]) - 图像旋转

		将当前图像，逆时针旋转给定角度。可定义旋转后，未覆盖区域的背景颜色！
		$angle - 旋转角度
		$bgcolor - 旋转后，可能出现未覆盖区域，我们可设置背景色

	11.mask($source [, bool $mask_with_alpha = false])
		将给定的图像资源应用为当前图像的alpha蒙板，以改变当前透明度。蒙板将调整为当前图像大小。默认情况下，蒙板的灰度版本会转换为alpha值，但可通过设置 $mask_with_alpha 以应用世纪的alpha通道。将保持当前图像的透明度。
		$source：
			string - 文件系统的图片路径
			string - 图片的URL地址(allow_url_fopen必须启用)
			string - 二进制图片数据
			string - data-url编码的图片数据
			string - base64编码的图片数据
			resource - gd类型的PHP资源(当使用GD库)
			object - Imagick实例(当使用Imagick库)
			object - Intervention\Image\Image 实例
			object - SplFileInfo instance (To handle Laravel file uploads via Symfony\Component\HttpFoundation\File\UploadedFile) - laravel框架自带的图片上传实例
		$mask_with_alpha - 设置为true，可将实际的alpha通道作为蒙板，应用于当前图像，替代颜色值。默认是 false

应用效果：	
	1.filter(Intervention\Image\Filters\FilterInterface $filter) - 使用滤镜

		给当前图像，应用自定义的滤镜效果

	2.blur($amount = 1) - 高斯模糊

		在当前图片上，应用高斯模糊滤镜。$amount可选范围为 0-100。(GD库处理，高强度的高斯模糊会非常占用性能，小心使用)

		示例：
			$img = Image::make('public/foo.jpg');
			$img->blur();		// 轻微的高斯模糊
			$img->blur(15);		// 高强度的高斯模糊

	3.sharpen($amount = 10) - 锐化效果

		锐化当前图像。$amount可选范围为 0-100。

		示例：
			$img = Image::make('public/foo.jpg');
			$img->sharpen(15);

	4.pixelate($size) - 像素化效果

		将像素化效果应用于当前图像(指定像素化效果的尺寸)

	5.limitColors($count [, $matte])

		将当前图片的现有颜色，转换为具有给定最大颜色数的颜色表。该函数保留尽可能多的alpha通道信息，并将透明像素与可选的遮罩颜色混合。
		$count - 应在调色板中保留的最大颜色数。设置为null，转为真彩色(truecolor)
		$matte - 用于混合透明像素的颜色。默认值：无遮罩颜色

		示例：
			$img = Image::make('public/foo.png');
			$img->limitColors(255, '#f90');

	6.interlace($interlace = true) - 图像隔行扫描

		传递一个boolean类型参数，切换隔行扫描模式，来确定是否使用隔行扫描或标准模式，对图像进行编码。如果jpeg图像使用隔行扫描模式，图像将被处理为渐进式jpeg。
		设置为true－隔行扫描模式｜false－标准模式。默认是true

		示例：
			$img = Image::make('public/foo.png');
			$img->interlace();			// 开启隔行扫描
			$img->save();
			$img = Image::make('public/interlaced.gif');
			$img->interlace(false);		// 关闭隔行扫描
			$img->save();


画图：
	1.text($text [, $x [, $y [, Closure $callback]]]) - 文字

		在指定的位置，写入文本。在回调函数中，可定义更多细节，例如：字体大小，字体文件，对其方式等
		$callback - 回调函数
			file($filepath) - 字体文件。设置True Type Font文件的路径，或者GD库内部字体之一的1到5之间的整数值。 默认值：1
			size($size) - 字体大小。字体大小仅在设置字体文件时可用，否则将被忽略。 默认值：12
			color($color) - 字体颜色
			align($align) - 水平对齐方式：left,right,center。默认left
			valign($valign) - 垂直对齐方式：top,bottom,middle。默认bottom
			angle($angle) - 文本旋转角度。文本将围绕垂直和水平对齐点逆时针旋转。 旋转仅在设置字体文件时可用，否则将被忽略

	2.pixel($color, $x, $y) - 点

		在给定的坐标上，以给定的颜色画单个像素点

	3.line($x1, $y1, $x2, $y2 [Closure $callback]) - 线

		在当前图像上，指定2点画线。可以在回调函数中，定义线的颜色和宽度
		$callback
			color($color) - 指定线的颜色。默认：#000000
			width($width) - 指定线的宽度。默认：1px。			// GD库无效！！

	4.rectangle($x1, $y1, $x2, $y2 [Closure $callback]) - 矩形

		指定2点坐标，分别代表左上角和右下角，来画一个矩形。

	5.polygon(array $points [, Closure $callback]) - 多边形

		使用给定的点数据，来画一个多边形。可以在回调函数中，定义多变形的外观
		$callback 可使用下面的方法，来定义圆的外观：
			background($color) - 定义圆的背景
			border($width, $color) - 定义圆的边框

		示例：
			$img = Image::canvas(800, 600, '#ddd');
			$points = array(
				40, 50,
				30, 40,
				20, 30,
				10, 20,
				5, 10,
			);
			$img->polygon($points, function($draw){
				$draw->background('#ff0');
				$draw->border(1, '#f0f');
			})

	6.circle($diameter, $x, $y [,Closure $callback]) - 圆

		在给定的x、y坐标点上，使用给定的直径画一个圆。
		可以通过回调函数，来定义圆的外观。
		$callback 可使用下面的方法，来定义圆的外观：
			background($color) - 定义圆的背景
			border($width, $color) - 定义圆的边框

		示例：
			$img = Image::canvas(300, 200, '#ddd');
			$img->circle(100, 50, 50, function($draw){
				$draw->background('#ff0');
				$draw->border(1, '#f00');
			})

	7.ellipse($width = 10, $height = 10, $x, $y [Closure $callback]) - 椭圆

		在给定的x、y坐标点，绘制彩色椭圆。可以定义宽度和高度，以及通过可选的闭包回调来设置椭圆的外观
		$callback 可使用下面的方法，来定义圆的外观：
			background($color) - 定义圆的背景
			border($width, $color) - 定义圆的边框


图像的其它处理方法：
	1.fill($filling [, $x, $y]) - 图像填充

		使用给定颜色或另一张图片来填充当前图像。传递可选的x、y坐标，表示开始填充的位置。
		如果指定了x、y坐标，原始图像上该坐标的颜色，将执行泛红填充(flood fill)。如果未指定坐标，不管下面是什么，填充整个图像

		$filling - 填充颜色或图像模式。图像资源格式如下：
			string - 文件系统的图片路径
			string - 图片的URL地址(allow_url_fopen必须启用)
			string - 二进制图片数据
			string - data-url编码的图片数据
			string - base64编码的图片数据
			resource - gd类型的PHP资源(当使用GD库)
			object - Imagick实例(当使用Imagick库)
			object - Intervention\Image\Image 实例
			object - SplFileInfo instance (To handle Laravel file uploads via Symfony\Component\HttpFoundation\File\UploadedFile) - laravel框架自带的图片上传实例

		示例：
			$img = Image::canvas(800, 600);
			$img->fill('#ccc');
			$img->fill('public/foo.jpg');
			$img->fill('#f0f', 0, 0);

	2.insert($source [, $position [, $x, $y]]) - 插入图片

		将一个给定的图片资源插入到当前图像中。

		$source - 另一个图片资源

		$position - 位置(相对于当前图像)
			top-left(默认)
			top
			top-right
			left
			center
			right
			bottom-left
			bottom
			bottom-right

		$x,$y - 偏移坐标(默认：0.0)
			在 $position 指定的位置上，再进行相对偏移

		此方法可用于，将另一张图像作为水印，因为保持了透明度。

		示例：
			$img = Image::make('public/foo.jpg');
			$img->insert('public/bar.png');									// 插入另一个图片
			$watermark = Image::make('public/watermark.png');				// 创建一个新的图片实例
			$img->insert($watermark, 'center');								// 插入图片
			$img->insert('public/watermark.png', 'bottom-right', 10, 10);

	3.pickColor($x, $y [string $format]) - 选取给定坐标颜色

		在当前图像上的指定坐标，获取颜色，并返回给定的颜色格式
		$format - 颜色格式，可以是：
			array - array(255, 255, 255, 1)
			rgb - rgb(255, 255, 255)
			rgba - rgba(255, 255, 255, 0.5)
			hex - #cccccc
			int - 16776956
		默认情况下，该方法将像素的RGB值作为数组返回。使用整数格式，将颜色直接传递给任意GD库函数

检索信息：
	1.width()
		获取当前图片的宽度(单位：px)

		示例：
			$width = Image::make('public/foo.jpg')->width();

	2.height()
		获取当前图片的高度(单位：px)

		示例：
			$height = Image::make('public/foo.jpg')->height();

	3.mime()
		如果已经图像已经定义了MIME类型，从当前图像实例中读取MIME类型。

	4.exif([string $key])
		从当前图像读取 'Exif' 元数据。必须从文件路径是实例化图像对象，才能正确读取EXIF数据
		imagick从2.3.9开始支持exif。PHP编译时，必须包含 '--enable-exif' 才能使用这个方法。windows用户，还必须开启 'mbstring' 扩展(PHP手册有该函数库)
		$key - 检索的exif哪个下标

	5.iptc([string $key])
		从当前图像中读取 'IPTC' 元数据
		默认获取所有元数据，可以传递下标，来获取指定的元数据。没有发现元数据，返回 'NULL'

		示例：
			$data = Image::make('public/foo.jpg')->iptc();						// 获取所有数据

			$copyright = Image::make('public/foo.jpg')->iptc('Copyright');		// 获取 'Copyright'

	6.filezise()
		如果实例是从一个真实文件初始化的，获取当前图像实例的文件尺寸
		返回图像的尺寸大小，单位是 'bytes - 字节'，如果不是从一个文件创建的图像实例，则返回false

		示例：
			$img = Image::make('public/foo.jpg');
			$size = $img->filesize();
```




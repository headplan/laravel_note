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




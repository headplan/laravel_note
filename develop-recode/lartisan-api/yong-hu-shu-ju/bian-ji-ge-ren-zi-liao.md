# 编辑个人资料

#### 数据的提交方式

HTTP 提交数据有两种方式

* application/x-www-form-urlencoded\(默认值\)
* multipart/form-data

form 表单提交文件的时候 , 需要增加`enctype="multipart/form-data"` , 才能正确传输文件 , 因为默认的`enctype`是`enctype="application/x-www-form-urlencoded"`

所以 , 只有当 POST 配合`multipart/form-data`时才能正确传输文件 . 

#### 图片资源

一般有关文件上传的接口 , 会设计为两个 , 因为涉及到文件的 , 例如要修改用户头像 , 必须使用 POST 的`multipart/form-data`

* 调用 POST api/images 在服务器创建图片资源
* 通过图片资源的 id 或路径 , 请求修改头像接口

添加一个图片资源

```
php artisan make:migration create_images_table --create='images'
```

修改文件

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateImagesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('images', function (Blueprint $table) {
            $table->increments('id');
            $table->integer('user_id')->index();
            $table->enum('type', ['avatar', 'topic'])->index();
            $table->string('path')->index();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('images');
    }
}
```




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

images 表记录了用户 id , 图片路径 , 以及图片类型 . 图片类型有两种 'avatar' 和 'topic' , 分别用于用户头像以及话题中的图片 . 记录图片类型是因为不同类型的图片有不同的尺寸 , 以及不同的文件目录 , 修改个人头像所使用的 image 必须为 avatar 类型 .

```
php artisan migrate
```

**添加路由**

routes/api.php

```php
# 需要 token 验证的接口
$api->group(['middleware' => 'api.auth'], function($api) {
    # 当前登录用户信息
    $api->get('user', 'UsersController@me')
        ->name('api.user.show');
    # 图片资源
    $api->post('images', 'ImagesController@store')
        ->name('api.images.store');
});
```

创建模型 , request 以及controller

```
php artisan make:model Models/Image
php artisan make:request Api/ImageRequest
php artisan make:controller Api/v1/ImagesController
```

修改_app\Models\Image.php_

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Image extends Model
{
    protected $fillable = ['type', 'path'];

    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```






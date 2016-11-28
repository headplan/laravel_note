# 验证码和session

**验证服务**

* 验证码验证 - gregwar\/captcha

* 验证码验证 - mews\/captcha : https:\/\/github.com\/mewebstudio\/captcha

* 滑块验证 - geetest

* 手机验证 - laravel-sms : https:\/\/github.com\/toplan\/laravel-sms


**手写验证码类**

引入类的步骤

* 创建app\Helpers文件夹
* 编辑composer.json文件,添加classmap或者在类中加入命名空间
  * classmap文件夹"app\/Helpers"
  * 自动加载命名空间:**namespace **App\Helpers\code;

* 执行命令composer update
* 创建路由`Route::`_`get`_`('admin/code', 'Admin\LoginController@code');`
* 创建方法
  ```
  public function code()
  {
      $code = new Code;
      $code->make();
  }
  ```

* 修改视图
* 添加验证码连接,这里使用了url\(\)函数,和设置css和js时一样使用asset\(\)函数
* 点击重新请求验证码onclick="**this**.src='{{ url\('admin\/code'\) }}?'+Math.random\(\)"
* 验证码获取一般会使用到SESSION,Laravel默认没有开启原生的SESSION,可以在入口文件开始session\_start\(\)


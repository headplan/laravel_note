# 认证

### 知识点整理

```
1.认证介绍
php artisan make:auth
php artisan migrate
生成用户登录注册所需要的所有东西

配置文件:config/auth.php
包含调整认证服务行为的选项

认证组件:guards和providers
Guard 定义了用户在每个请求中如何实现认证
Provider 定义了如何从持久化存储中获取用户信息(还可以定义额外的Provider)

数据库考量
app目录下默认包含了 Eloquent 模型App\User(当然也可以切换为database认证驱动,利用查询构造器)
其中两个字段
password 字段长度至少有60位,默认字串255
remember_token 字段可以为空的、字符串类型的,字段长度为100,用于在登录时存储被应用维护的“记住我”的Session令牌

2.快速入门
预置的认证控制器
App\Http\Controllers\Auth下
RegisterController:新用户注册
LoginController:用户登录认证
ForgotPasswordController:重置密码邮件链接
ResetPasswordController:包含重置密码逻辑
每个控制器都使用 trait 来引入它们需要的方法

路由
php artisan make:auth自动生成,还会生成个HomeController

视图
resources/views/auth目录下,自动生成

认证
控制器默认已经包含了注册及登录逻辑(通过trait)
    自定义路径:protected $redirectTo = '/';下面三个控制器中都有这个属性,认证成功跳转的路径
        LoginController,RegisterController,ResetPasswordController
        也可以定义个redirectTo方法完成逻辑后跳转,优先级大于属性.
    自定义用户名:
        在LoginController中定义username()方法
        public function username()
        {
            return 'username';
        }
    自定义Guard:
        自定义用于实现用户注册登录的“guard”.
        在LoginController,RegisterController和ResetPasswordController中定义guard方法,该方法返回一个实例
        use Illuminate\Support\Facades\Auth;

        protected function guard()
        {
            return Auth::guard('guard-name');
        }
    自定义验证/存储:
        RegisterController 的 validator 方法包含了新用户注册的验证规则
        RegisterController 的 create 方法负责使用 Eloquent ORM 在数据库中创建新的 App\User 记录
获取认证用户

路由保护

登录失败次数限制

3.手动认证用户

4.基于HTTP的基本认证

5.添加自定义的Guard

6.添加自定义用户提供者

7.事件
```




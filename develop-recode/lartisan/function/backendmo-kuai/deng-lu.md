# 登录

**创建登录控制器**

```
php artisan make:controller Backend/Auth/BackendLoginController
```

* AdminLoginController - 先来看看LoginController这个控制器的实体 , 在use AuthenticatesUsers中 , 前面已经提到了.
  * 路由访问的是showLoginForm方法和login方法
  * 所以创建Admin的登录控制器`php artisan make:controller Auth/AdminLoginController`
  * 创建上面两个方法showLoginForm和login , 先完成show方法展示页面 . 提交action绑定到login方法
  * 初始化配置中间件`guest:admin`
  * 绑定路由
  * 再来看看login方法的步骤
    * 验证form表单数据 , 也就是email和password字段
    * 尝试登陆 , 这里用到了前面的自定义认证`Auth::guard('admin')->attempt($credentials, $remember)`
      * 其中password字段被排除了trim过滤 , 可以查看trim全局中间件
    * 登陆成功重定向跳转 , 这里使用了intended\(\)方法 , 表示将用户请求重定向至被用户验证过滤器拦截之前用户试图访问URL中去 . 
    * 认证失败重定向到上一页并带着提交的数据
    * 更多逻辑后续添加
  * 使用tinker创建admin数据 , 登录成功
  * 重写logout , 先新建一个component视图 , 区分logout的是user还是admin .
    * 默认的login控制器自带了一个logout , 但是会清空所有的session记录 , 这里分别重写
    * logout和userLogout , 添加路由
    * 分别在控制器的guest中间件中排除掉logout方法




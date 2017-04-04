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

```




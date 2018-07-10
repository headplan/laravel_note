# 微信登录功能开发

#### 调整用户表结构

首先需要为 users 表增加两个字段 , weixin\_openid , weixin\_unionid , 用来记录微信用户的唯一标识 . 修改 password 字段为 nullable , 因为第三方登录不需要密码 . 

```
php artisan make:migration add_weixin_openid_to_users_table --table=users
```




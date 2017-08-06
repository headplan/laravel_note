# Security\_Auth

快速启动认证系统只需要两个命令

```
php artisan make:auth
php artisan migrate
```

Migrate是创建database/migrations下的迁移 . 当然创建之前 , 要配置数据库连接在.env文件中 . 这里有几点要注意 :

* 创建数据库表结构时 , 确认密码字段最少必须 60 字符长 , 保持255即可
* `users`数据表中必须含有 nullable  , 100 字符长的`remember_token`字段 , 这是给**记住我**这个功能用的

> Learning\_Laravel的测试代码中 , 添加了多用户的控制 , 下面的内容都会根据原Auth命令生成的基础上修改添加 .

认证系统的配置文件`config/auth.php`, 这个后面在详细说明 .

---

先看一下生成的文件


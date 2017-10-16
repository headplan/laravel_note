# 部署上线

Laravel的部署 , 参考官方的文档即可 . 

#### 服务器要求

Laravel框架对PHP版本和扩展有一定要求，不过这些要求 Homestead 都已经满足了，不过如果你没有使用 Homestead 的话（那真是一件很遗憾的事情），有必要了解下这些以便确认自己的环境满足要求：

* PHP 
  &gt;
  = 7.0.0
* PHP OpenSSL 扩展
* PHP PDO 扩展
* PHP Mbstring 扩展
* PHP Tokenizer 扩展
* PHP XML 扩展

满足以上需求之后，就可以开始安装Laravel 了。

项目初始化在Homstead中生成很方便 : 

```
composer create-project --prefer-dist laravel/laravel blog 5.4.*
```

#### 配置Laravel

web服务器指向public目录 . 

可以在config中配置

`storage`和`bootstrap/cache`的目录权限给755即可

```
# 权限遵循 文件644 文件夹755 
# 权限用户和用户组www
chown -R www.www /data/wwwroot/
find /data/wwwroot/ -type d -exec chmod 755 {} \;
find /data/wwwroot/ -type f -exec chmod 644 {} \;
```




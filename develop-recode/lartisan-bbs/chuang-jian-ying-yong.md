# 创建应用

前面已经配置好了基本的开发环境\(Homestead\) .

**配置Composer加速 : **

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

**创建Laravel应用 : **

```
composer create-project laravel/laravel ./ --prefer-dist "5.5.*"
```

**配置本地Hosts : **

```
192.168.66.66 lartisan.bbs
```

前面已经配置过了yaml文件 , 直接访问即可 .

**配置.env文件 : **

```
APP_ENV=local # 本地的开发环境
APP_DEBUG=true # 设定是否在应用报错时显示调试信息
APP_KEY=your_app_key # 生成应用的密钥用于加密一些较为敏感的数据

# 对数据库的连接方式、数据库名、用户名密码等做相关配置
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=lartisan_bbs
DB_USERNAME=homestead
DB_PASSWORD=secret

# 缓存、会话、队列等驱动的相关配置信息
CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync

# Redis配置
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# 邮件相关配置
MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```

调用`getenv('APP_ENV')`将返回`local`


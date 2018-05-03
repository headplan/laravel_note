# 配置文件

#### 配置env文件

根据不同环境配置 :

* 本地开发环境
* 测试环境
* 生产环境

```
APP_ENV   # 来设定当前应用的运行环境local,testing,production
APP_DEBUG # 来设定是否在应用报错时显示调试信息
APP_KEY   # 来生成应用的密钥用于加密一些较为敏感的数据
```

对数据库的连接方式、数据库名、用户名密码等做相关配置 :

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sample
DB_USERNAME=homestead
DB_PASSWORD=secret
```

缓存、会话、队列等驱动的相关配置信息 :

```
CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync
```

本地完整配置

```
APP_NAME=Lartisan
APP_ENV=local
APP_KEY=base64:USg2BvkNDtZFT4UQ2eOPoVLc5unNbtM0PCjqFZF7S6E=
APP_DEBUG=true
APP_LOG_LEVEL=debug
APP_URL=http://lartisan.local

DB_CONNECTION=mysql
DB_HOST=192.168.66.66
DB_PORT=3306
DB_DATABASE=lartisan
DB_USERNAME=homestead
DB_PASSWORD=secret

BROADCAST_DRIVER=log
CACHE_DRIVER=file
SESSION_DRIVER=file
SESSION_LIFETIME=120
QUEUE_DRIVER=sync

REDIS_HOST=192.168.66.66
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1
```

---

#### 配置config/app.php

基本配置都在env文件中 , 这里暂时只修改UTC时区和本地语言 .

```
'timezone' => 'PRC',
'locale' => 'zh',
```




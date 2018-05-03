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

| 文件名称 | 配置类型 |
| :--- | :--- |
| app.php | 应用相关，如项目名称、时区、语言等 |
| auth.php | 用户授权，如用户登录、密码重置等 |
| broadcasting.php | 事件广播系统相关配置 |
| cache.php | 缓存相关配置 |
| database.php | 数据库相关配置，包括 MySQL、Redis 等 |
| filesystems.php | 文件存储相关配置 |
| mail.php | 邮箱发送相关的配置 |
| queue.php | 队列系统相关配置 |
| services.php | 放置第三方服务配置 |
| session.php | 用户会话相关配置 |
| view.php | 视图存储路径相关配置 |

#### 配置config/app.php

基本配置都在env文件中 , 这里暂时只修改UTC时区和本地语言 .

```
'name' => env('APP_NAME', 'Laravel'),
'env' => env('APP_ENV', 'production'),
'debug' => env('APP_DEBUG', false),
'url' => env('APP_URL', 'http://localhost'),
'timezone' => 'Asia/Shanghai',
'locale' => 'zh-CN',
```




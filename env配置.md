### ENV配置

```
APP_ENV=local # 运行环境的名称
APP_DEBUG=true # 调试模式开启
# 使用命令php artisan key:generate重新生成秘钥
APP_KEY=base64:Igs1BdzHI9XIV1clE1+aELqlvccO7sIGuX55++c6ghU= # 敏感信息加密的秘钥,例如session等.
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```


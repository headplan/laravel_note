### ENV配置

```
APP_ENV=local # 运行环境的名称
APP_DEBUG=true # 调试模式开启
# 使用命令php artisan key:generate重新生成秘钥
APP_KEY=base64:Igs1BdzHI9XIV1clE1+aELqlvccO7sIGuX55++c6ghU= # 敏感信息加密的秘钥,例如session等.
APP_URL=http://localhost # 项目URL地址

# 数据库配置
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret

缓存驱动配置
CACHE_DRIVER=file # 缓存驱动,file表示文件模式缓存
SESSION_DRIVER=file # session缓存
QUEUE_DRIVER=sync # 队列,使用sync模式

# Redis配置
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# 邮件服务器配置
MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```

.gitignore中已经排除了git提交.env这个文件

```
/vendor
/node_modules
/public/storage
Homestead.yaml
Homestead.json
.env
```

上面这些文件和文件件都已经忽略提交


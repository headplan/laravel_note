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

上面这些文件和文件件都已经忽略提交.所以可以将一些重要配置信息写入.env文件,不进行提交.其他配置都写入到config目录下的配置文件中,这里引入了env函数,函数有两个参数,第一个参数是引入.env中常量的配置项,如果.env文件中没有配置,则使用第二个参数为默认值.

例子,将config文件夹中database.php里的mysql配置项中的表前缀项,写到.env文件中,配置文件中使用env获取.env中的参数

```
# .env文件添加
DB_PREFIX=blog_
# database.php文件修改成
'prefix' => env('DB_PREFIX', ''),
```

要获取config文件夹下的配置项,可以使用config函数




# 缓存系统

### 知识点整理

```
1.配置信息
配置文件config/cache.php
默认使用将序列化缓存对象保存在文件系统中的file缓存驱动
推荐使用Memcached和Redis
"apc", "array", "database", "file", "memcached", "redis"

# 驱动前提条件
    数据库缓存
    php artisan cache:table // 生成migration文件

    Memcached
    使用Memcached驱动需要安装Memcached PECL扩展包
    'memcached' => [
        [
            'host' => '127.0.0.1',
            'port' => 11211,
            'weight' => 100
        ],
    ],

    Redis
    安装predis/predis扩展包或者PhpRedis PHP 拓展

2.缓存的使用
    # 获取一个缓存实例
        
    # 从缓存中获取项目
    # 存放项目到缓存中
    # 删除缓存中的项目
    # Cache帮助函数

3.缓存标签

4.增加自定义的缓存驱动

5.缓存事件
```




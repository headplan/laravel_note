# Redis

### 知识点整理

```
1.简介
    安装包:composer require predis/predis
    替代方案:通过PECL安装PHP扩展PhpRedis(安装较麻烦,如果Redis应用较多性能更好)
        https://github.com/phpredis/phpredis
# 配置
    'redis' => [
        'client' => 'predis',
        'default' => [
            'host' => env('REDIS_HOST', 'localhost'),
            'password' => env('REDIS_PASSWORD', null),
            'port' => env('REDIS_PORT', 6379),
            'database' => 0,
        ],
    ],
    # 配置集群
        集群将会在节点之间进行客户端分区,从而允许构建节点池并创建大量可用内存.
        客户端分片并不处理故障转移,非常适合从另一个主数据存储那里获取有效的缓存数据
        使用本地Redis集群,需要在Redis配置的options中进行指定.
        'redis' => [
            'client' => 'predis',
            'options' => [
                'cluster' => 'redis',
            ],
            'clusters' => [
                // ...
            ],
        ],

    Predis和PhpRedis都支持额外的用于定义每个Redis服务器的连接参数,写在配置文件即可.
    使用PhpRedis需要在Redis配置中将client选项修改为phpredis.

2.与Redis交互
    use Illuminate\Support\Facades\Redis;
    Redis::命令,接Redis命令即可,再传递参数.或者使用,
    Redis::command,第一个参数是命令,第二个是传递的参数.
# 使用多个Redis连接
    $redis = Redis::connection();
    $redis = Redis::connection('配置中的服务器名或集群名');
    
# 管道命令
    在一次操作中发送多个命令到服务器的时候应该使用管道.
    使用pipeline方法接收一个接收Redis实例的闭包,来处理多条命令的之心:
    Redis::pipeline(function ($redis) {
        for ($i = 0; $i < 1000; $i++) {
            $redis->set("key:$i", $i);
        }
    });
    
3.发布/订阅
    就是调用Redis的publish和subscribe命令的接口.
    使用subscribe方法通过Redis在一个频道上设置监听器
    调用subscribe方法会开启一个常驻进程,在Artisan命令中调用该方法.
    public function handle()
    {
        Redis::subscribe(['test-channel'], function($message) {
            echo $message;
        });
    }
    使用publish发布消息到该频道
    Route::get('publish', function () {
        // 路由逻辑...
        Redis::publish('test-channel', json_encode(['foo' => 'bar']));
    });
    
# 通配符订阅
    使用psubscribe方法可以订阅到一个通配符定义的频道
    这在所有相应频道上获取所有消息时很有用.
    $channel名将会作为第二个参数传递给提供的回调闭包:
    Redis::psubscribe(['*'], function($message, $channel) {
        echo $message;
    });
    
    Redis::psubscribe(['users.*'], function($message, $channel) {
        echo $message;
    });
```




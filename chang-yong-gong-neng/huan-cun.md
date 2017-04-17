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
        Illuminate\Contracts\Cache\Factory - 为应用程序定义了访问所有缓存驱动的机制
        Illuminate\Contracts\Cache\Repository - 用cache配置信息文件指定你的应用程序默认缓存驱动的实现
        也可以使用Cache facade访问缓存实例
        Cache::get('key');
        ## 访问多个缓存仓库
        Cache::store('file')->get('foo');
        Cache::store('redis')->put('bar', 'baz', 10);

    # 从缓存中获取项目
        Cache facade中的get方法用来从缓存中获取缓存项
        如果缓存中不存在该缓存项,返回 null
        可以向get方法传递第二个参数,用来指定缓存项不存在时返回的默认值
        $value = Cache::get('key');
        $value = Cache::get('key', 'default');
        还可以使用闭包,缓存不存在返回闭包中的结果.
        $value = Cache::get('key', function () {
            return DB::table(...)->get();
        });

        ## 确认项目是否存在
        if (Cache::has('key')) {)

        ## 递增与递减值
        Cache::increment('key');
        Cache::increment('key', $amount);
        Cache::decrement('key');
        Cache::decrement('key', $amount);

        ## 获取和更新
        例如,从缓存中取出所有用户,或者当用户不存在时,从数据库中将这些用户取出并放入缓存中
        $value = Cache::remember('users', $minutes, function () {
            return DB::table('users')->get();
        });

        ## 获取和删除
        从缓存中获取一个缓存项然后删除它.缓存不存在返回null
        $value = Cache::pull('key');

    # 存放项目到缓存中
        $expiresAt = Carbon::now()->addMinutes(10);
        Cache::put('key', 'value', $expiresAt);

        ## 写入目前不存在的项目
            add方法只会把暂时不存在于缓存中的缓存项放入缓存
            Cache::add('key', 'value', $minutes);

        ## 永久写入项目
        forever方法可以用来将缓存项永久存入缓存中
        forget方法手动删除
        Cache::forever('key', 'value');
        注:Memcached驱动缓存达到大小限制,永久的也会被删除      

    # 删除缓存中的项目
        Cache::forget('key');
        Cache::flush(); // 清空所有缓存,注意共享缓存也会被清空

    # Cache帮助函数
        $value = cache('key');
        cache(['key' => 'value'], $minutes);
        cache(['key' => 'value'], Carbon::now()->addSeconds(10));

3.缓存标签
    缓存标签并不支持使用file或dababase的缓存驱动
    # 写入被标记的缓存项
    Cache::tags(['people', 'artists'])->put('John', $john, $minutes);
    # 访问被标记的缓存项
    $john = Cache::tags(['people', 'artists'])->get('John');
    # 移除被标记的缓存项
    Cache::tags(['people', 'authors'])->flush(); // 两个标记都删除
    Cache::tags('authors')->flush(); // 只删除一个标记的缓存

4.增加自定义的缓存驱动
    # 写驱动
    参考Illuminate\Cache\MemcachedStore
    # 注册
    Cache::extend的调用会在最新的Laravel应用程序默认的App\Providers\AppServiceProvider的boot方法中完成
    或者可以创建自己的服务提供者来放置这些扩展,然后在config/app.php中注册即可.然后在config/cache.php中配置即可.
    <?php

    namespace App\Providers;

    use App\Extensions\MongoStore;
    use Illuminate\Support\Facades\Cache;
    use Illuminate\Support\ServiceProvider;

    class CacheServiceProvider extends ServiceProvider
    {
        /**
         * Perform post-registration booting of services.
         *
         * @return void
         */
        public function boot()
        {
            Cache::extend('mongo', function ($app) {
                return Cache::repository(new MongoStore);
            });
        }

        /**
         * Register bindings in the container.
         *
         * @return void
         */
        public function register()
        {
            //
        }
    }
5.缓存事件
在EventServiceProvider中监听.
protected $listen = [
    'Illuminate\Cache\Events\CacheHit' => [
        'App\Listeners\LogCacheHit',
    ],

    'Illuminate\Cache\Events\CacheMissed' => [
        'App\Listeners\LogCacheMissed',
    ],

    'Illuminate\Cache\Events\KeyForgotten' => [
        'App\Listeners\LogKeyForgotten',
    ],

    'Illuminate\Cache\Events\KeyWritten' => [
        'App\Listeners\LogKeyWritten',
    ],
];
```




# **用户切换工具 sudo-su**

https://github.com/viacreative/sudo-su

```
# 安装
composer require "viacreative/sudo-su:~1.1"
# 配置
php artisan vendor:publish --provider="VIACreative\SudoSu\ServiceProvider"
```

使用方法

```
class AppServiceProvider extends ServiceProvider
{
    public function register()
    {
        if (config('app.debug')) {
            $this->app->register('VIACreative\SudoSu\ServiceProvider');
        }
    }
}

@if (config('app.debug'))
    @include('sudosu::user-selector')
@endif
```




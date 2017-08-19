# Laratrust

https://github.com/santigarcor/laratrust

http://laratrust.readthedocs.io/en/4.0/

**安装**

```
composer require "santigarcor/laratrust:4.0.*"
```

**配置config/app.php\(如果是Laravel5.5版本则不需要\)**

```
# providers
Laratrust\LaratrustServiceProvider::class,

# aliases
'Laratrust'   => Laratrust\LaratrustFacade::class,
```

**生成配置文件**

```
php artisan vendor:publish --tag="laratrust"
```

> 如果生成失败执行清空即可
>
> ```
> php artisan config:clear
> ```




# 创建应用

前面已经配置好了基本的开发环境\(Homestead\) .

**配置Composer加速 : **

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

**创建Laravel应用 : **

```
composer create-project laravel/laravel ./ --prefer-dist "5.5.*"
```

**配置本地Hosts : **

```
192.168.66.66 lartisan.bbs
```

前面已经配置过了yaml文件 , 直接访问即可 . 

**配置.env文件 : **

```
DB_DATABASE=lartisan_bbs
```




# DingoAPI

Dingo是Larave和Lumen都可用的 RestFul 工具包 . 文档地址 :

* [https://github.com/dingo/api/wiki](https://github.com/dingo/api/wiki) - 文档
* [https://github.com/liyu001989/dingo-api-wiki-zh](https://github.com/liyu001989/dingo-api-wiki-zh) - 中英对照文档

Github - [https://github.com/dingo/api](https://github.com/dingo/api)

#### 安装

编辑配置文件 , composer.json , 添加

```
"dingo/api": "2.0.0-alpha1"
```

```
composer update
```

#### 配置

选择对应的配置 :

```
php artisan vendor:publish
```

生成了config/api.php文件 , 下面是需要用到的配置选项 :

```
API_STANDARDS_TREE=prs
API_SUBTYPE=lartisan
API_PREFIX=api
API_VERSION=v1
API_DEBUG=true
```

**API\_STANDARDS\_TREE  ,  API\_SUBTYPE**

通过Accept头来指定需要访问的API版本 , 这两个配置指定的是 : 

```
Accept: application/<API_STANDARDS_TREE>.<API_SUBTYPE>.v1+json
```

API\_STANDARDS\_TREE 有是三个值可选 : 

* `x`本地开发的或私有环境的
* `prs`未对外发布的 , 提供给公司 app , 单页应用 , 桌面应用等
* `vnd`对外发布的 , 开放给所有用户

这里使用`psr`选项 . 

API\_SUBTYPE 一般情况下是项目的简称 : 

```
API_SUBTYPE=lartisan
```

现在访问不同版本的格式是 : 

```
访问 v1 版本
Accept: application/prs.lartisan.v1+json
访问 v2 版本
Accept: application/prs.lartisan.v2+json
```

**API\_PREFIX , API\_DOMAIN**

同一个项目 , 可以通过前缀或者子域名的方式来区分开 API 与 Web 等页面访问地址 : 

```
API_PREFIX=api # 通过前缀的方式
```

```
API_DOMAIN=api.larabbs.com # 通过子域名的方式
```

**API\_VERSION**

默认的API版本 , 当没有传`Accept`头的时候 , 默认访问的版本 , 这里配置为v1即可 : 

```
API_VERSION=v1
```

**API\_STRICT**

是否开启严格模式 . 开启 , 则必须使用`Accept`头才可以访问API , 也就是说必须通过POSTMAN或其他工具设置`Accept`后 , 才可以访问API , 根据需求设置 , 这里暂时false . 

**API\_DEBUG**

测试环境打开DEBUG , 查看错误信息 , 方便定位错误 . 


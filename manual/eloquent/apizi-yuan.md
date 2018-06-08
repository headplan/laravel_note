# API资源

构建API时通常需要一个中间连接层 , 去连接Eloquent模型和实际返回给用户的json响应 . 这里直接创建Larave资源类 , 即可快速构建 .

#### 生成资源

使用artisan命令生成 , 默认生成在`app/Http/Resources`文件夹下 , 继承`Illuminate\Http\Resources\Json\Resource`类 .

```php
php artisan make:resource User
```

**资源集合**

除了生成资源转换单个模型外 , 还可以生成资源集合用来转换模型的集合 . 它允许在响应中包含与给定资源相关的链接与其他元信息 .

生成方式有两种 , 都可以 :

```php
php artisan make:resource Users --collection

php artisan make:resource UserCollection
```

都表明生成的是一个资源集合 . 




# API资源

构建API时通常需要一个中间连接层 , 去连接Eloquent模型和实际返回给用户的json响应 . 这里直接创建Larave资源类 , 即可快速构建 . 

#### 生成资源

使用artisan命令生成 , 默认生成在`app/Http/Resources`文件夹下 , 继承`Illuminate\Http\Resources\Json\Resource`类 . 

```php
php artisan make:resource User
```




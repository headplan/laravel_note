# 数据模型

基本操作前文有记录 , 这里记录一些流程备份的东西 . 

生成表 : 

```
php artisan migrate
```

创建移动模型 : 

```
$ mkdir app/Models
$ mv app/User.php app/Models/User.php
```

移动后 , 模型文件修改 : 

```
namespace App\Models;
```

搜索修改的命名空间 . 

提交刚才的修改 . 

```
$ git add -A
$ git commit -m "Move user model to models folder"
```




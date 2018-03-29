# 对象操作

> 使用 Laravel 提供的 Tinker 环境来操作对象 , Tinker 是一个 REPL \(read-eval-print-loop\) , REPL 指的是一个简单的、可交互式的编程环境 , 通过执行用户输入的命令 , 并将执行结果直接打印到命令行界面上来完成整个操作 . 这里的Tinker封装了Psy Shell包 , 可以查阅官网[http://psysh.org/](http://psysh.org/)

**进入Tinker环境**

```
$ php artisan tinker
```

**创建用户对象**

```
>>> App\Models\User::create([
    'name'=> 'hello', 
    'email'=>'hello@email.com',
    'password'=>bcrypt('password')
])
```

**查找用户对象**

```
>>> use App\Models\User
>>> User::find(1)
>>> User::find(5)
=> null
>>> User::findOrFail(5)
Illuminate\Database\Eloquent\ModelNotFoundException with message 'No query results for model [App\Models\User] 5'
>>> User::first()
>>> User::all()
```

**更新用户对象**

```
>>> $user = User::first()

>>> $user->name = 'Headplan'
>>> $user->save() // 调用 save 方法对用户信息进行保存
>>> User::first()
=> App\Models\User {#687
     id: 1,
     name: "Headplan",
     email: "hello@email.com",
     created_at: "2006-09-01 08:53:33",
     updated_at: "2006-09-01 08:57:33",
   }
   
>>> $user->update(['name'=>'hello']) // 直接调用 update 方法进行更新
```

**相关文档**

https://laravel-china.org/docs/laravel/5.5/eloquent


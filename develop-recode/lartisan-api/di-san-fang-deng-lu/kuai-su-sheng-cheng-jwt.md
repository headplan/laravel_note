# 快速生成JWT

**主要为调试目的** , 现在可以通过登录 , 第三方登录 , 创建一个`token`了 , 需要用户身份认证的接口 , 都需要我们携带`token`进行访问 . 这时候就会有一个问题 , `token`是有过期时间的 , 我们每次要调试接口时都需要重新创建一个`token` ; 尝试不同身份用户调用的时候 , 就需要重为不用的用户创建`token`.

#### 增加 command

```
php artisan make:command GenerateToken
```

编辑命令文件

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use App\Models\User;

class GenerateToken extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'lartisan:generate-token';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = '快速生成用户token';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        $userId = $this->ask('输入用户 id');
        $user = User::find($userId);
        if (!$user) {
            return $this->error('用户不存在');
        }

        # 一年以后过期
        $ttl = 365*24*60;
        $this->info(\Auth::guard('api')->setTTL($ttl)->fromUser($user));
    }
}
```

**测试命令**

```
php artisan lartisan:generate-token
```

使用postman保存 . 给postman增加变量 , 用来存放这个token . 以后方便使用 . 之后输入`{`就可以提示 . 


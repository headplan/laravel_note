# Laravel的Auth重新

```
namespace App\Http\Controllers\Auth;

# 继承自Controller;
use App\Http\Controllers\Controller;
# 这里用到了一个Trait
use Illuminate\Foundation\Auth\AuthenticatesUsers;
# 初始化引入了中间件guest   
'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,

trait AuthenticatesUsers;
这个Trait主要完成了基本的登录逻辑,其中引入了两个Trait.

```




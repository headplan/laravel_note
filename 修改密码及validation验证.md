### 修改密码

**创建路由**

```
Route::any('pass', 'IndexController@pass');
```

把普通的路由都改为get方法

**创建控制器方法**

```
# 判断是否post数据,并复制给$input
if ($input = Input::all()) {
    # 添加验证规则
    $rules = [
        'password' => 'required|between:6,20|confirmed',
    ];
    # 添加验证规则返回信息
    $msg = [
        'password.required' => '新密码不能为空',
        'password.between' => '新密码6-20位',
        'password.confirmed' => '新密码和确认密码不一致',
    ];
    # 引入Validator门面
    $validator = Validator::make($input, $rules, $msg);
    # passes()方法判断输入信息是否通过上面的规则
    if ($validator->passes()) {
        # 获取数据库数据
        $user = User::first();
        # 解密数据库中取出的密码
        $_userpass = Crypt::decrypt($user->userpass);
        # 判断原密码是否输入正确
        if ($input['password_o'] == $_userpass) {
            # 赋值新密码
            $user->userpass = Crypt::encrypt($input['password']);
            # 更新新密码到数据库
            $user->update();
            # 返回提示信息,同样返回给$errors
            return back()->with('errors', '密码修改成功');
        } else {
            # 返回提示信息,同样返回给$errors
            return back()->with('errors', '原密码输入错误');
        }
    } else {
        # 把上面没通过验证的错误信息输出到视图,用$errors接收
        return back()->withErrors($validator);
    }
}
# 默认使用模板
return view('admin.pass');
```

**引入模板**

```
@extends('layout.admin')
@section('content')
        <div class="result_wrap">
        <div class="result_title">
            <h3>修改密码</h3>
            @if(count($errors) > 0)
                <div class="mark">
                    // 这里判断$errors是对象,代表是withErrors()返回的验证规则信息
                    @if(is_object($errors))
                        @foreach($errors->all() as $error)
                            <p>{{ $error }}</p>
                        @endforeach
                    @else
                        <p>{{ $errors }}</p>
                    @endif
                </div>
            @endif
        </div>
    </div>
@endsection
```




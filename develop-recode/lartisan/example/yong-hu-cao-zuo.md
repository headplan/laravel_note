# 用户操作

**更新用户**

前面用户展示已经使用了resource资源路由 , 不用添加编辑路由了

```php
Route::get('/users/{user}/edit', 'UsersController@edit')->name('users.edit');
```

这里也使用了隐性路由模型绑定 , 控制器直接写即可

```php
public function edit(User $user)
{
    return view('users.edit', compact('user'));
}
```




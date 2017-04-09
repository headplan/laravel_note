# 哈希

### 知识点整理

```
1.Laravel的Hash门面为存储用户密码提供了安全的Bcrypt哈希算法
2.基本使用
可以调用Hash门面上的make方法对存储密码进行哈希
Hash::make($request->newPassword)
通过哈希验证密码
Hash::check('plain-text', $hashedPassword)
检查密码是否需要被重新哈希
needsRehash方法允许判断哈希计算器使用的工作因子在上次密码被哈希后是否发生改变
if (Hash::needsRehash($hashed)) {
    $hashed = Hash::make('plain-text');
}
```




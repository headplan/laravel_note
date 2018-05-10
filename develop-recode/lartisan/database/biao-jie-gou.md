# 表结构

#### users

```
Schema::create('users', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
# 添加了两个字段
Schema::table('users', function (Blueprint $table) {
    $table->string('avatar')->nullable();
    $table->string('introduction')->nullable();
});
```




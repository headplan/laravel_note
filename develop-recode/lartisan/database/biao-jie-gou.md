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
# 添加了三个个字段
Schema::table('users', function (Blueprint $table) {
    $table->string('avatar')->nullable();
    $table->string('introduction')->nullable();
    $table->enum('gender', ['male', 'female', 'unselected'])->default('unselected');
});
```

#### categories

```
Schema::create('categories', function (Blueprint $table) {
    $table->increments('id');
    $table->integer('parent_id')->default(0)->comment('父级ID');
    $table->integer('post_count')->default(0)->comment('帖子数');
    $table->tinyInteger('weight')->default(0)->comment('权重');
    $table->string('name')->index('categories_name_index')->null()->comment('名称');
    $table->string('slug', 60)->unique('categories_slug_unique')->null()->comment('缩略名');
    $table->string('description')->nullable()->comment('描述');
    $table->timestamps();
    $table->softDeletes();
});
```






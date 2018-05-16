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

#### topics

```
Schema::create('topics', function (Blueprint $table) {
    $table->increments('id');
    $table->string('title')->index()->comment('帖子标题');
    $table->text('body')->comment('帖子内容');
    $table->text('excerpt')->nullable()->comment('帖子摘要');
    $table->string('slug')->index()->nullable()->comment('英文url');    
    $table->integer('user_id')->unsigned()->index()->default(0)->comment('用户ID');
    $table->integer('category_id')->unsigned()->index()->default(0)->comment('分类ID');
    $table->integer('reply_count')->unsigned()->index()->default(0)->comment('回复数量');
    $table->integer('view_count')->unsigned()->index()->default(0)->comment('查看总数');
    $table->integer('vote_count')->unsigned()->index()->default(0)->comment('投票总数');
    $table->integer('last_reply_user_id')->unsigned()->index()->default(0)->comment('最后回复的用户ID');
    $table->integer('order')->unsigned()->index()->default(0)->comment('排序');
    $table->tinyInteger('topic_status')->index()->default(0)->comment('0:默认;1:精华;2:锁回复;3:标记;4:草稿');
    $table->timestamps();
    $table->softDeletes();
});

php artisan make:scaffold Topic --schema="title:string:index,body:text,user_id:integer:unsigned:index,category_id:integer:unsigned:index,reply_count:integer:unsigned:default(0),view_count:integer:unsigned:default(0),last_reply_user_id:integer:unsigned:default(0),order:integer:unsigned:default(0),excerpt:text,slug:string:nullable"
```




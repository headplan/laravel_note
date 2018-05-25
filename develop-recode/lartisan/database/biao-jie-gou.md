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

#### categories - 主要记录主分类,暂无二级分类,预留.

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

#### topics - 记录内容 , 包括帖子和专栏文章等.

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

#### articles - 记录专栏分类

```
Schema::create('articles', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name')->index()->comment('名称');
    $table->string('slug')->index()->comment('别名');
    $table->string('description')->comment('描述');
    $table->string('cover')->comment('封面');
    $table->integer('user_id')->unsigned()->index()->comment('用户ID');
    $table->integer('article_count')->unsigned()->index()->default(0)->comment('文章数总数');
    $table->integer('article_status')->default(0)->comment('0:默认私有专栏;1:公共专栏;2:推荐专栏;3:锁定专栏无法访问');
    $table->timestamps();
    $table->softDeletes();
});
```

#### article\_topics - 专栏分类ID与帖子\(即内容\)ID

```
Schema::create('article_topics', function (Blueprint $table) {
    $table->integer('article_id')->unsigned()->index()->comment('文章ID');
    $table->integer('topic_id')->unsigned()->index()->comment('帖子,即内容ID');
    $table->foreign('article_id')->references('id')->on('articles')->onDelete('cascade');
    $table->foreign('topic_id')->references('id')->on('topics')->onDelete('cascade');
});
```




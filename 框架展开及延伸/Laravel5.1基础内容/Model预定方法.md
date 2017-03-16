# Model预定方法

这里介绍两种方式,使Model更加丰富,同时使控制器更加简洁明了.

**set{字段名}Attribute , 字段入库前的处理**

```
# 编辑Model
public function setPublishAtAttribute($date)
{
    $this->attributes['publish_at'] = Carbon::createFromFormat('Y-m-d', $date);
}
# 在视图中添加publish_at
<div class="form-group">
    {!! Form::label('publish_at', '发布:') !!}
    {!! Form::input('date', 'publish_at', null, ['class' => 'form-control']) !!}
</div>
# 控制器直接提交所有数据
$post = $request->all();
Article::create($post);
```

**拆分where条件为独立方法,约定scope{方法名}**

```
# 创建Model中的条件方法
public function scopePublishAt($query)
{
    $query->where('publish_at', '<=', Carbon::now());
}

# 在控制器中使用
$articles = Article::latest()->publishat()->get();
```

**Carbon对象简单介绍**

```
# 系统创建的时间都是Carbon对象,好处是可以直接获取改变里面的东西
$article->created_at->year;
# 显示一天前,十分钟前等...
$article->created_at->diffForHumans());

# 要使一个自定义字段以Carbon对象展示,在Model中定义个属性即可
protected $dates = ['publish_at'];
```




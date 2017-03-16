# Model预定方法

这里介绍两种方式,使Model更加丰富,同时使控制器更加简洁明了.

set{字段名}Attribute , 字段入库前的处理

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




# 话题模块

```
# 使用生成器创建话题模块骨架,整理字段,重新生成表,具体查看表结构
# 填充话题假数据
编辑数据工厂TopicFactory
编辑数据填充TopicsTableSeeder
填充数据
编辑Model模型,定义关联关系
关联分类
关联用户

php artisan vendor:publish --tag=laravel-pagination
分页改成pjax ---------------------------------------------->todo
各个分类列表信息由分类控制器接管.
修改顶部分类选中is-actice
创建本地作用域,展示不同排序数据
页面展示.
添加用户个人中心数据展示.关联topic模型.hasMany(),一对多关系.

# 话题的CURD
新增添加话题入口,导航一个,右侧边栏一个.
编辑fillable字段.
控制器,传topic.
编辑页面,页面中判断是create还是edit.
填充各个字段fill();
保存之前切一部分内容作为excerpt,放在observer中.
然后注册到App服务中.
在TopicRequest卡一下输入权限.
中间件登录限制,show除外.

# 引入编辑器SimpleMDE编辑器
生成editor独立文件管理样式与js
修改全屏样式,提到最前z-index:99999.
markdown样式未修改---------------------------------------------->todo
开启js本地缓存,缓存key为路由+更新时间
添加title标题的缓存.
预览代码高亮使用highlight.js,样式Atom One Dark
预览追加wysiwyg类.
toolbar样式图标改为淘宝图标.
编辑器拖拽复制上传图片,参考github:
https://github.com/sparksuite/simplemde-markdown-editor/issues/328
https://github.com/Laravel-Backpack/CRUD/issues/535
```



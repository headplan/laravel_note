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
添加title标题的缓存.localForage.
预览代码高亮使用highlight.js,样式Atom One Dark
预览追加wysiwyg类.
toolbar后两个自定义图标改为淘宝图标.自定义跳转链接和click提交按钮
编辑器拖拽复制上传图片,参考github:
https://github.com/sparksuite/simplemde-markdown-editor/issues/328
https://github.com/Laravel-Backpack/CRUD/issues/535
emoji图标解析暂未添加--------------------------------------------->todo

# 创建内容优化
title简单过滤e();
添加body_markdown字段,存储默认的markdown内容.
修改Model的fillable,修改seed数据生成相关内容.
添加markdown与html互转的插件,添加过滤html插件.创建Handler类,管理.
composer require league/html-to-markdown -> html to markdown
composer require erusev/parsedown -> markdown to html
其他描述查看Handler类.
添加client_platform字段,存储客户端平台信息手机IOS,安卓.
创建helper获取Request::header('X-Client-Platform');
内容图片在非定义格式内容上传默认图片,记录日志中--------------------------------->todo
slug别名添加翻译,创建Handler类,管理.
安装Guzzle HTTP套件.
安装PinYin翻译扩展.
如果没有则翻译slug别名.
创建新的路由,添加别名字段.
在Model中创建新的方法,调整新的路由格式.拼接id和slug.
修改控制器中的调用为to($topic->link())
修正show中的url地址,判断slug别名部位空并且请求的别名和实际别名不等则301从定向到前面的方法.
```




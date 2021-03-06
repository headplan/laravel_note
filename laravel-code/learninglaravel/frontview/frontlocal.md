# Front\_Local

前面的Blade标签中@lang和@choice已经了解了本地化的一些功能 , 这里进一步完善一下 .

Laravel 的本地化功能提供方便的方法来获取多语言的字符串 , 存放在resources/lang目录下 , tree是这样的

```
/resources
    /lang
        /en
            messages.php
        /es
            messages.php
```

语言包简单地返回键值和字符串数组 . 默认语言配置在config/app.php中 , local和fallback\_locale . 如果想要动态的切换 , 有一组facade操作 :

* App::setLocale\(\) - 动态设置语言
* App::getLocale\(\) - 获取语言设置
* App::isLocale\(\) - 判断语言是啥

### 语言包

前面已经提到了在 resources/lang目录下定义语言包 , 可以创建目录 , 目录名就是语言包的名字 , 里面是php文件 . 还可以直接在resources/lang目录下定义json文件 , 这种方式常用在有大量翻译需求的应用 , 使用翻译语言作为长键 . 例如

```
{
    "I love programming.": "Me encanta la programación."
}
```

语言包中的内容获取 , 前面的Blade模板中介绍了两种 , 对应的全局函数也有相同的 , 更能和参数是一样的

* \_\_\('msg.test',\[\],'cn'\) 和trans\('msg.test',\[\],'cn'\) - 这里要注意的是trans\(\)函数不支持json文件的语言包
* @lang
* trans\_choice\('msg.test',1,\[\],'cn'\) - 这个函数也是无法加载json文件语言包的
* @choice

语言包中的参数替换使用:冒号 , 多选复数使用\|分割 , 和前面Blade模板中提到的一样 , 这里不再复述 . 

**重写扩展包的语言包**

部分扩展包带有自己的语言包 , 可以通过在`resources/lang/vendor/{package}/{locale}`放置文件来重写它们 , 而不是直接修改扩展包的核心文件 . 


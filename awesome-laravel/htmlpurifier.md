# HTMLPurifier

**白名单机制**

[http://htmlpurifier.org/](http://htmlpurifier.org/)

[https://github.com/mewebstudio/Purifier](https://github.com/mewebstudio/Purifier)

```
# 安装
composer require "mews/purifier:~2.0"
# 配置
php artisan vendor:publish --provider="Mews\Purifier\PurifierServiceProvider"
```

常见配置

```
<?php

return [
    'encoding'      => 'UTF-8',
    'finalize'      => true,
    'cachePath'     => storage_path('app/purifier'),
    'cacheFileMode' => 0755,
    'settings'      => [
        'user_topic_body' => [
            'HTML.Doctype'             => 'XHTML 1.0 Transitional',
            'HTML.Allowed'             => 'div,b,strong,i,em,a[href|title],ul,ol,ol[start],li,p[style],br,span[style],img[width|height|alt|src],*[style|class],pre,hr,code,h2,h3,h4,h5,h6,blockquote,del,table,thead,tbody,tr,th,td',
            'CSS.AllowedProperties'    => 'font,font-size,font-weight,font-style,margin,width,height,font-family,text-decoration,padding-left,color,background-color,text-align',
            'AutoFormat.AutoParagraph' => true,
            'AutoFormat.RemoveEmpty'   => true,
        ],
    ],
];
```

可以配置更多 , 使用时 , 指定clean\(\)方法的第二个参数即可user\_topic\_body . 

```
$topic->body = clean($topic->body, 'user_topic_body');
```




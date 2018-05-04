# 中文拼音转换

https://github.com/overtrue/pinyin

```
composer require "overtrue/pinyin:~3.0"
```

#### 拼音数组

```
use Overtrue\Pinyin\Pinyin;

// 小内存型
$pinyin = new Pinyin(); // 默认
// 内存型
// $pinyin = new Pinyin('Overtrue\Pinyin\MemoryFileDictLoader');
// I/O型
// $pinyin = new Pinyin('Overtrue\Pinyin\GeneratorFileDictLoader');


$pinyin->convert('带着希望去旅行，比到达终点更美好');
// ["dai", "zhe", "xi", "wang", "qu", "lv", "xing", "bi", "dao", "da", "zhong", "dian", "geng", "mei", "hao"]

$pinyin->convert('带着希望去旅行，比到达终点更美好', PINYIN_UNICODE);
// ["dài","zhe","xī","wàng","qù","lǚ","xíng","bǐ","dào","dá","zhōng","diǎn","gèng","měi","hǎo"]

$pinyin->convert('带着希望去旅行，比到达终点更美好', PINYIN_ASCII);
//["dai4","zhe","xi1","wang4","qu4","lv3","xing2","bi3","dao4","da2","zhong1","dian3","geng4","mei3","hao3"]
```

* 小内存型: 将字典分片载入内存
* 内存型: 将所有字典预先载入内存
* I/O型: 不载入内存，将字典使用文件流打开逐行遍历并运用php5.5生成器\(yield\)特性分配单行内存

三种字符

* 默认不带音调 , PINYIN\_NONE
* 带UNICODE音调 , PINYIN\_UNICODE
* 带数字音调 , PINYIN\_ASCII


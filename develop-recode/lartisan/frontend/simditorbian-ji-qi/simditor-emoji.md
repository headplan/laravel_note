# simditor-emoji

```
yarn add simditor-emoji
# npm上版本还是2.1.0
# js引入也有问题.这里直接从官方github下载引入
```

#### 配置

```
new Simditor({
    textarea: textareaElement,
    ...,
    toolbar: [..., 'emoji'],
    emoji: {
        imagePath: 'images/emoji/'
    }
})
```

图片加载路径只能写死 , 而且图片总是全部加载 . 暂不使用 , 改为替换方案 , 


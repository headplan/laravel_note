# 生产环境

生产环境建议使用自定义的docker-compose.yml文件 , 而且这个自定义的生产环境的yml文件应该只包含准备在生产环境上运行的容器 , 然后运行这个配置文件 : 

```
docker-compose -f production-docker-compose.yml up -d nginx mysql redis ...
```

> 注意: 数据库 \(mysql/MariaDB/...\) 端口不应在生产环境中转发 , 因为 "Docker" 将自动在主机上发布端口 , 这是相当不安全的 , 除非明确告诉你不要这样做 . 因此, 请务必删除这些行:
>
> ```
> ports:
>     - "3306:3306"
> ```

关于Docker如何发布端口 , 可以查看这篇文章 : 

https://fralef.me/docker-and-iptables.html


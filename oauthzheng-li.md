# OAuth整理

参考资料

> [http://www.rfcreader.com/\#rfc6749](http://www.rfcreader.com/#rfc6749)
>
> [http://blog.csdn.net/s1070/article/details/51174655](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)

**整合App note笔记和Laravel的API认证系统Passport中的内容.**

OAuth是一个关于授权\(authorization\)的开放网络标准,在全世界得到广泛应用,目前的版本是2.0版.

### **目录**

```
1.应用场景
    传统认证缺点
        应用为了后续的服务,会保存用户的密码.
        另一个应用需要部署密码登录.
        用户无法限制应用获取自己在另一个应用中获取资料,授权范围和有效期.
        用户只有修改密码,才能收回权限.
        用户授权是整体的,一个被破解,所有其他应用都会丢失.
2.名词定义
    
3.OAuth的思路
4.运行流程
5.客户端的授权模式
6.授权码模式
7.简化模式
8.密码模式
9.客户端模式
10.更新令牌
```




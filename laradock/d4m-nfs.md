# 安装配置D4m-nfs扩展

`https://github.com/IFSight/d4m-nfs`

通过D4m-nfs可以把docker的`file sharing`挂载到本地.

首先配置Docker,打开Preferences中`file sharing`的配置,删除其中多余的目录,只保留/tmp目录.

然后,克隆d4m-nfs到`~`目录下

```
git clone https://github.com/IFSight/d4m-nfs ~/d4m-nfs
```

项目克隆下来后修改`~/d4m-nfs/etc/d4m-nfs-mounts.txt`文件, 若文件不存在自己手动建一个, 加入要挂在的目录 : 

```
/Users:/Users
/Volumes:/Volumes
/private:/private
```






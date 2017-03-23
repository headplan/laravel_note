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

检查系统文件`/etc/exports`下是否有内容, 编辑文件清空所有内容

在`~`目录下运行脚本

```
~/d4m-nfs/d4m-nfs.sh
```

> 脚本运行后程序开始初始化最后终端会展示以下内容
>
> ```
> ....
> Please note:
> * To connect to the D4M moby linux VM use: screen -r d4m
> * To disconnect from the D4M moby linux VM tty screen session use Ctrl-a d.
> * To run d4m-nfs faster 
> and
> /
> or
>  offline, leave the files 
> in
>  d4m-apk-cache 
> and
>  the hello-world image.
> * If you 
> switch
>  between D4M stable 
> and
>  beta, you might need to remove files 
> in
>  d4m-apk-cache 
> and
>  the hello-world image.
>  ....
> ```

通过命令进入挂载目录的窗口

```
screen -r d4m
```

如果看到以下挂载项表示挂载成功

```
192.168.65.1:/Users/Lavekin /mnt nfs nolock,local_lock=all 0 0
192.168.65.1:/Users /Users nfs nolock,local_lock=all 0 0
192.168.65.1:/Volumes /Volumes nfs nolock,local_lock=all 0 0
192.168.65.1:/private /private nfs nolock,local_lock=all 0 0
```

回到`laradock`目录下将容器跑起来

```
docker-compose up -d nginx redis mysql
```




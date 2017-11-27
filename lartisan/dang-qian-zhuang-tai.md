# 当前状态

### 配置好了本地环境

如需修改可参考环境搭建Homestead中的相关资料 .

### 使用Git版本控制 , 初始化第一个版本Initial commit .

#### **Git配置**

进入Homestead虚拟机中 , 配置Git用户名和邮箱 :

```
git config --global user.name "WebModel"
git config --global user.email headplan@163.com
```

设置 Git 推送分支时相关配置 , 执行`git push`没有指定分支时 , 自动使用当前分支 , 而不是报错 :

```
git config --global push.default simple
```

**Git初始化**

```
cd ~/Code/Laravel
git init
```

添加所有文件到Git中\(当然是除了`.gitignore`忽略的\)

```
git add -A
```

提交

```
git status
git commit -m "Initial commit"
git log
```

> 如果有误删 , 可以`git checkout -f`恢复 , 更多可以参考Git相关Note

### 同步到远程\(本地宿主机\)中的Gogs统一管理 . \(宿主:192.168.10.1\)

#### Gogs配置

Homestead在初始化时 , 通过 Homestaed.yaml 文件中的 `keys`选项 , 我们已经把专门的Homestead的 SSH Key 私钥复制到虚拟机中 , 这里需要将 SSH Key 添加到 ssh-agent 中 :

    $ eval `ssh-agent -s`
    $ ssh-add ~/.ssh/homestead

然后将公钥添加到 Gogs 账号即可 .

### 通过walle拉取Gogs中的版本部署到线上服务器 . 

walle配置可以查看部署上线中walle的安装和Git项目配置 . 其中会出现问题的通常是目录权限问题 .

项目配置分为 : 

* 测试环境 - 绑定本地host
* 线上环境 - 绑定域名

高级任务 : 

```
# post_deploy代码检出之后
# 复制vendor仓库:因为宿主在本地这里直接复制
cp -r {WORKSPACE}/../../package/vendor {WORKSPACE}/vendor
# 生成.env文件:这里的env配置分为线上线下(正式和测试),即online和offline
cp {WORKSPACE}/../../package/env/offline.env {WORKSPACE}/.env

# post_release代码同步并穿件链接之后
# 生成App Key
php artisan key:generate
```




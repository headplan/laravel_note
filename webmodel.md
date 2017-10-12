# WebModel

前面已经在Homestead中安装好了Laravel , 开始之前还需要简单配置一些东西 .

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

#### **Github配置**

Homestead在初始化时 , 通过 Homestaed.yaml 文件中的 `keys`选项 , 我们已经把专门的Homestead的 SSH Key 私钥复制到虚拟机中 , 这里需要将 SSH Key 添加到 ssh-agent 中 : 

    $ eval `ssh-agent -s`
    $ ssh-add ~/.ssh/homestead

然后将公钥添加到 GitHub 账号即可 . 


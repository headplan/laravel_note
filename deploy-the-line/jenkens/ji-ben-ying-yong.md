# Jenkins介绍

Jenkins是基于java开发的一种持续集成工具 , 用于监控持续重复的工作 , 功能包括 :

* 持续的软件版本发布/测试
* 监控外部调用执行项目

Jenkins最近又收到关注的原因是Devops备受关注 , 如何来做持续集成 , 持续交付 , 如何来做CI/CD . Jenkins作为持续集成的工具 , 他其实只是一个平台或者是一个大的框架 , 它的工作完全就是依靠插件 , 也就是说你想使用什么功能 , 你就找到什么样的插件 .

#### **Jenkins的好处**

在工作中部署Jenkins的最大好处就是每次在开发、测试环境代码 , 都无须运维部署 , 而是相关的开发人员 , 测试人员登录Jenkins传入需要部署的tag即可 , 整个部署过程无须运维参与 , 解放运维劳动力 .

安卓 , IOS自动打包 . 虽然打包和运维关系不大 , 但是运维实现自动打包 , 使得产品同学 , 运营和测试同学可以每日验证产品开发进度以及及时反馈开发功能的方向是否正确 , 对公司贡献还是不小的 .

#### **Jenkins的实践**

通常都会有自动化部署的shell脚本和上线流程 , 使用Jenkins主要是为了 , 让开发,测试人员可以通过web界面 , 方便操作执行脚本 , 实现部署的动作 .

**常见产品线有四种环境 :**

开发环境 , 测试环境 , 预上线环境 , 生产环境 . 通常情况下 , 除了生产环境 , 都可以通过Jenkins部署 .

**常见的部署方式 :**

触发式构建 : 常用在开发环境的部署 , 开发人员push代码或者合并代码到Git项目的master分支 , Jenkins就部署代码到对应服务器 .

参数化构建 : 常用于测试环境和预上线环境部署 , 开发人员push代码或者合并代码到Git项目的master分支之后 , 并不会部署代码 , 而是需要登录到Jenkins的web界面 , 点击构建按钮 , 传入对应的参数\(比如 , 参数需要构建的tag , 需要部署的分支\) , 然后才会部署 .

定时构建 : 常用于APP自动打包 , 定时构建是在参数化构建的基础上添加的 , 开发人员可以登录Jenkins手动传入tag进行打包 , 如果不手动打包 , 那么Jenkins就每天凌晨从Git拉取最新的APP代码打包 .

> 有了定时构建等 , 还可以去定时的备份一些数据 , 或者同步生产环境数据库到开发环境等操作 , 都可以在Jenkins中构建 .

#### Jenkins使用注意

* 需要各个部门推动 , 比如APP自动打包 . 
* 确定的代码分支管理规范 . 可以参考 : Git 分支管理最佳实践 - [http://www.ibm.com/developerworks/cn/java/j-lo-git-mange/](http://www.ibm.com/developerworks/cn/java/j-lo-git-mange/)
* 实现过通过shell自动部署代码 . 
* 持续集成最好有自动化测试 , 最好是让开发人员提供api的监控脚本 , 每次构建之后验证部署是否正常 . 




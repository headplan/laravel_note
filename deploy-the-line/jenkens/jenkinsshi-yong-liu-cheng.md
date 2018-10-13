# Jenkins使用流程

安装流程查看介绍中的相关内容 .

安装完成后 , 开始将Jenkins运行并创建Pipeline .

**Jenkins Pipeline**是一套插件 , 支持将连续输送Pipeline实施和整合到Jenkins . Pipeline提供了一组可扩展的工具 , 用于将"复制代码"作为代码进行建模 .

**Jenkinsfile**是一个包含Jenkins Pipeline定义的文本文件 , 并被检入源代码控制 . 这是"Pipeline代码"的基础 . 处理连续输送Pipeline的一部分应用程序 , 以像其他代码一样进行版本检查 . 创建Jenkinsfile提供了一些直接的好处 :

* 自动创建所有分支和拉请求的Pipeline
* Pipeline上的代码审查/迭代
* Pipeline的审计跟踪
* Pipeline的唯一真实来源 , 可以由项目的多个成员查看和编辑 . 

虽然在Web UI或a中定义Pipeline的语法和Jenkinsfile是相同的 , 但通常认为最佳做法是在Jenkinsfile中定义Pipeline并检查源控制 .

#### 创建任务

**新建任务**

输入一个任务名称 , 例如 , 我的Pipeline . 

**构建一个自由风格的软件项目**

这是Jenkins的主要功能 . Jenkins将会结合任何SCM和任何构建系统来构建你的项目 , 甚至可以构建软件以外的系统 . 

**流水线**

精心地组织一个可以长期运行在多个节点上的任务 . 适用于构建流水线\(更加正式地应当称为工作流\) , 增加或者组织难以采用自由风格的任务类型 . 

**构建一个多配置项目**

适用于多配置项目 , 例如多环境测试 , 平台指定构建 , 等等 .




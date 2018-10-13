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






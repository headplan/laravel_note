# Laravel 5.3

## 目录结构

### **简介**

默认的目录结构试图为不管大型应用还是小型应用提供一个好的起点,可以按照自己的喜好重新组织应用目录结构,Laravel对类在何处被加载没有任何限制,只要Composer可以自动载入即可.

**Models目录**

Laravel没有Models目录,因为models容易造成歧义,有的人认为模型指的是业务逻辑,有的则认为模型指的是关联数据库的交互.所以,默认将Eloquent的模型直接放置到App目录下,从而允许开发者自行选择放置的位置.

### 根目录

* App目录
* Bootstrap目录
* Config目录
* Database目录
* Public目录
* Resources目录
* Routes目录
* Storage目录
* Tests目录
* Vendor目录

### App目录说明




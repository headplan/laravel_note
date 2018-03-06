# Repository模式

##### 扩展

[https://github.com/bosnadev/repository](https://github.com/bosnadev/repository)

[https://github.com/andersao/l5-repository](https://github.com/andersao/l5-repository)

##### 参考资料

[http://laravelacademy.org/post/3053.html](http://laravelacademy.org/post/3053.html)

[http://laravelacademy.org/post/3063.html](http://laravelacademy.org/post/3063.html)

### 模式定义

Repository是一个独立的层,介于领域层与数据映射层\(数据访问层\)之间.它的存在让领域层感觉不到数据访问层的存在,它提供一个类似集合的接口提供给领域层进行领域对象的访问.Repository是仓库管理员,领域层需要什么东西只需告诉仓库管理员,由仓库管理员把东西拿给它,并不需要知道东西实际放在哪.

Repository模式是架构模式,在设计架构时,才有参考价值.应用Repository模式所带来的好处,远高于实现这个模式所增加的代码.只要项目分层,都应当使用这个模式.

Repository是衔接数据映射层和领域层之间的一个纽带,作用相当于一个在内存中的域对象集合.客户端对象把查询的一些实体进行组合,并把它们提交给Repository.对象能够从Repository中移除或者添加,就好比这些对象在一个Collection对象上进行数据操作,同时映射层的代码会对应的从数据库中取出相应的数据.

> Repository是把一个数据存储区的数据给封装成对象的集合并提供了对这些集合的操作,感觉以前都会写到Model层中

![](/assets/import.png)

这种将数据访问从业务逻辑中分离出来的模式有很多好处:

* 集中数据访问逻辑使代码易于维护
* 业务和数据访问逻辑完全分离
* 减少重复代码
* 使程序出错的几率降低

### Repository的实现

#### 第一步 - 接口

假定有两个数据对象,这两个数据对象上都有如下操作

* 获取所有记录
* 获取分页记录
* 创建一条新的记录
* 通过主键获取指定记录
* 通过属性获取相应记录
* 更新一条记录
* 删除一条记录

创建一个Repository接口

```
interface RepositoryInterface {
    public function all($columns = array('*'));
    public function paginate($perPage = 15, $columns = array('*'));
    public function create(array $data);
    public function update(array $data, $id);
    public function delete($id);
    public function find($id, $columns = array('*'));
    public function findBy($field, $value, $columns = array('*'));
}
```



